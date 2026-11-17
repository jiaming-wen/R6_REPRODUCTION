# Why these files are needed

[中文版](README.zh-CN.md)

| File | Purpose |
|---|---|
| r2_arc.py | Arc fitting used by r6_perceive |
| r5_grasp_v3.py | Camera parameters, geometry and template functions shared by perception and grasping |
| r6_snap.py | Captures stereo images and robot state |
| r6_calib.py | Calibration of the current setup |
| r6_beta.py | Beta measurement; moves the robot |
| r4_ecm_move.py | ECM framing motion |
| r6_fs_infer.py | Runs inference in an already configured FoundationStereo environment |
| r6_trial_start.sh | Creates an attempt record; `--nofs` records only the start pose and snap0 and does not call the old remote FS |
| r6_fs_remote.sh | Old laptop entry point, kept as the original; not used on the 3090 |
| r6_reuse_perception.py | Reuses perception for the same placement and checks for movement; assumes historical absolute paths |

All of these are unmodified original files. Add this directory to `PYTHONPATH` before running the main scripts; the exact command is in the protocol.
