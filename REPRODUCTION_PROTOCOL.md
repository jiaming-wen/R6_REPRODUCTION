# Reproduction protocol

[中文版](REPRODUCTION_PROTOCOL.zh-CN.md)

## A. Replay the two examples first (no robot)

Run inside `R6_REPRODUCTION/` with a Python environment that has numpy, opencv and scipy. These commands write only to a temporary directory and do not modify `examples/`.

```bash
set -euo pipefail
export P="$PWD"
export PYTHONPATH="$P/scripts/required_dependencies${PYTHONPATH:+:$PYTHONPATH}"
RUN=$(mktemp -d)
export RUN
mkdir -p "$RUN/trials"
cp -R examples/successful_trial "$RUN/trials/successful_trial"
cp -R examples/failed_trial "$RUN/trials/failed_trial"

# Expand the saved npz disparity into the npy the original script expects; only the working copy is written.
python3 - <<'PY'
import os
from pathlib import Path
import numpy as np
for p in Path(os.environ['RUN']).rglob('fs_disp.npz'):
    np.save(p.with_suffix('.npy'), np.load(p)['disp'].astype(np.float32))
PY

T="$RUN/trials/successful_trial"
python3 scripts/r6_perceive.py --trial "$T" \
  --snaps "$T/snap0" "$T/snap1" --clicks "$T/clicks.json" --s 0.5

T="$RUN/trials/failed_trial"
python3 scripts/r6_perceive.py --trial "$T" \
  --snaps "$T/perception_reference/snap0" "$T/perception_reference/snap1" \
  --clicks "$T/clicks.json" --s 0.25

echo "Results: $RUN/trials"
```

Inspect `perception.json` and `overlay_*.png` in the working copy and compare with the `p_star_mm` saved in `examples/`. The replay gate is a difference of at most 0.1 mm; bit-identical output is not expected because of float16 storage and the numerical environment. It is normal for the failed example's perception to pass: its failure happened during grasp execution.

**This is a replay of historical data, not fresh perception on site. Never pass it to `r6_grasp.py`.**

## B. In the lab: check the R5 environment on the RTX 3090

Use the existing FoundationStereo environment on the GPU workstation. First confirm the source, weights, config and caches listed in [requirements.md](requirements.md). Do not overwrite an existing installation. Then record the Python/PyTorch/CUDA versions and the model SHA256.

On the workstation, run the original inference script on a new set of images; fill in the actual paths on site:

```bash
# FS_PYTHON: python of the existing FS environment; FS_ROOT: source directory.
# FS_CKPT: 23-51-11/model_best_bp2.pth; IN/OUT: this run's input and a new output directory.
PYTHONPATH="$FS_ROOT" "$FS_PYTHON" \
  "$P/scripts/required_dependencies/r6_fs_infer.py" \
  --in_dir "$IN" --out_dir "$OUT" --ckpt "$FS_CKPT"
```

IN must contain `left/f.png` and `right/f.png`: put **topic right_rect.png** in left and **topic left_rect.png** in right. Copy the output `disp/f.npy` to the matching snapshot as `fs_disp.npy`. Run the two frames separately, keep each one's metadata, and never mix disparities between them.

Confirm that this frame produces valid output before continuing with on-site capture. This check has not yet been done on the 3090.

## C. Real-robot grasp (one phase at a time, on site)

Complete [SAFETY_CHECKLIST.md](SAFETY_CHECKLIST.md) first, then start the dVRK and cameras following the [original procedure](https://github.com/jiaming-wen/suturing-policy-sim2real/blob/aae23cf23a9b2a05fb139bcf0ce1524ccff0e5a3/docs/daily/2026-10-16/R6_grasp_pipeline.md) (in Chinese).

1. **Calibrate.** Use `r6_calib.py` and `r6_beta.py` in `required_dependencies`, following the original procedure: manifest → probe → grid → fit → check2. Continue only after the current calibration passes its check. The fixed read paths and required files are listed in requirements.md; the example directory is not deployed automatically.
2. **Start a new attempt.** On the robot side, run `bash "$P/scripts/required_dependencies/r6_trial_start.sh" "$T" --nofs` to save `start.json` and `snap0`, then take the second frame with `r6_snap.py "$T/snap1" -k 6`. `T` must be a newly created trial directory for this attempt.
3. **Compute perception.** Send both image pairs to the 3090, compute FS as in section B, and copy each result back to its snapshot. On `snap0/right_rect.png`, click a polyline from the needle tail to the tip and save `clicks.json` (see the examples for the format). Run `r6_perceive.py` with the same arguments as in section A but with this attempt's `T`. Confirm that every check passes.
4. **Approach and template.** Run the `r6_grasp` phases approach (condition C) → wrist → open, one at a time, confirming each before the next. On the left and right images at `open25`, click the gripper features and the midpoint of the jaw tips and save `template_clicks.json` (see the examples); then run template.
5. **Grasp.** Run hover → descend → touch → close_lift, one at a time; check whether the needle has moved before closing. Stop if any phase fails. The original procedure used `--mm 6` for touch; the operator decides whether it applies to the current setup.
6. **Finish.** Run putdown and release as the situation allows. Record success, failure or abort with the reason, and keep the logs and images.

Single-phase command format. The operator enters each phase by hand; **do not chain them automatically**:

```text
python3 "$P/scripts/r6_grasp.py" --trial "$T" --phase <phase>
approach additionally takes --cond C; for touch, see step 5.
```

Set the `PYTHONPATH` from section A on the robot side as well. The original script does not read `--config` and does not use `calibration_example` automatically; the current calibration must be at the fixed location listed in requirements.md.

## D. What to send back

For every attempt: the input stereo images and disparity, `clicks.json`, `perception.json`, `template_clicks.json`, `trial_state.json`, `trial_log.jsonl`, `preclose_pose.json`, the images before closing, after closing and after lifting, and the outcome label with the reason for any failure. For the first environment check, also include the versions and the weight hashes.
