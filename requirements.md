# What you need

[中文版](requirements.zh-CN.md)

## 1. Replaying the bundled examples

Only Python 3, numpy, opencv-python and scipy. No GPU, ROS, robot, FoundationPose or RL is needed.

The package includes the left and right images, the operator clicks, the saved FoundationStereo (FS) disparity, and the two code dependencies `r2_arc.py` and `r5_grasp_v3.py`. Set `PYTHONPATH` as in the protocol; no imports need to be changed.

The reduced historical data are stored as `fs_disp.npz` with key `disp`, in full-resolution pixels. The original `r6_perceive.py` reads `fs_disp.npy`, so the protocol expands the disparity only inside a temporary working copy.

Environment used to verify the examples: Python 3.14.2, numpy 2.3.5, opencv-python 4.13.0.92, scipy 1.16.3. This is a record of what was verified, not a requirement to replace the existing environment on the RTX 3090 workstation.

## 2. Recomputing FS on the RTX 3090 workstation

| Required | Current status |
|---|---|
| FoundationStereo source with a working CUDA/PyTorch environment | Check the existing R5 installation in the lab |
| `pretrained_models/23-51-11/model_best_bp2.pth` | Check the file and its hash on the workstation; not included here |
| The `cfg.yaml` paired with the weights | Must be in the same directory as the weights |
| imageio, omegaconf and FoundationStereo's own dependencies | Use the original project environment; do not install only from the imports in `r6_fs_infer.py` |
| DINOv2 Torch Hub source and timm EdgeNeXt pretrained cache | May be downloaded on first start; prepare them in advance for offline use |

This package includes `scripts/required_dependencies/r6_fs_infer.py`. Call it directly after setting FoundationStereo's `PYTHONPATH`; the input and output directories are passed as arguments.

`r6_fs_remote.sh` is the original laptop's SSH/WSL entry point and contains old machine paths. It is kept only as an original dependency. **Do not use it to connect to the 3090.**

## 3. Running on the real robot

- The lab dVRK, PSM1 with a ProGrasp 400093, the ECM stereo camera, and an operator on site.
- ROS 2 Jazzy on the robot side with a built `~/ros2_ws`: dvrk_robot, dvrk_video, dvrk_config_jhu.
- Python ROS modules: rclpy, sensor_msgs, geometry_msgs, std_msgs, crtk_msgs, cv_bridge, message_filters; plus numpy, opencv and scipy.
- Calibration and beta files for the current placement, fresh images, `clicks.json` and `template_clicks.json`. The historical calibration is only a format example.

The original `r6_grasp.py` reads these fixed paths:

```text
~/suturing-policy-sim2real/records/logs/R6_20261016/
├── calib/manifest.json
├── calib/calibration_r6.json
└── beta/beta_sweep.json
```

Without changing the code, **currently valid files** must be provided at this location. If historical files are already there, the operator must check them first; they must not be overwritten automatically with the example. The R2 TCP constants in the script are also tied to the original instrument.

**Still unverified:** whether the 3090's R5 environment runs this version of FS as is, whether the current calibration is valid, and whether ROS communication works on site. These can only be confirmed in the lab.
