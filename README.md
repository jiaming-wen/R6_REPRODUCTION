# R6 Grasp Reproduction

[中文版](README.zh-CN.md)

Materials for reproducing the R6 real-robot needle grasp on the da Vinci Research Kit (dVRK): the original scripts, the dependencies they need, a calibration example, and data from one successful and one failed trial.

The grasp uses FoundationStereo depth, an operator-clicked needle polyline and gripper template, and geometric control. FoundationPose and the RL policy are not part of this online grasp.

## Getting started

```bash
git clone https://github.com/jiaming-wen/R6_REPRODUCTION.git
cd R6_REPRODUCTION
```

1. Check the dependencies in [requirements.md](requirements.md).
2. Replay the bundled examples with section A of [REPRODUCTION_PROTOCOL.md](REPRODUCTION_PROTOCOL.md). This needs no GPU, ROS or robot.
3. To reproduce on the real robot, verify the GPU environment, calibrate the current setup and complete [SAFETY_CHECKLIST.md](SAFETY_CHECKLIST.md) before following the on-site steps of the protocol.

## Layout

```text
R6_REPRODUCTION/
├── README.md
├── requirements.md
├── scripts/
│   ├── r6_perceive.py
│   ├── r6_grasp.py
│   └── required_dependencies/
├── configs/
│   └── calibration_example/
├── examples/
│   ├── successful_trial/       # Q1_C
│   └── failed_trial/           # T3b_C
├── SAFETY_CHECKLIST.md
└── REPRODUCTION_PROTOCOL.md
```

## Examples and verification

Each example contains the images, the operator clicks, the saved FoundationStereo disparity and the execution record, so perception can be replayed without downloading the full experiment data.

| Example | Recorded grasp outcome | Replayed grasp point vs. archived |
|---|---|---|
| [Q1_C](examples/successful_trial/README.md) | Success | 0.00094 mm |
| [T3b_C](examples/failed_trial/README.md) | Failure | 0.00029 mm |

Both pass the 0.1 mm perception-replay gate. **Passing the perception replay does not mean the physical grasp succeeds.** The perception input that T3b_C reused from T3_C is included with the failed example.

Code, data and calibration files are byte-for-byte copies of the originals; only the reproduction notes and checklists are new. The source and SHA256 of every copied file are listed in [COPY_MANIFEST.csv](scripts/required_dependencies/COPY_MANIFEST.csv). This repository keeps the upstream [MIT License](LICENSE).

GPU inference and the current real-robot setup still have to be verified on site. The historical calibration is a reference for format and results only and cannot be used for a new placement of the setup. Fixed paths and model dependencies that are not included are listed in requirements.md.

Original pipeline: [R6 grasp pipeline](https://github.com/jiaming-wen/suturing-policy-sim2real/blob/aae23cf23a9b2a05fb139bcf0ce1524ccff0e5a3/docs/daily/2026-10-16/R6_grasp_pipeline.md) (in Chinese).
