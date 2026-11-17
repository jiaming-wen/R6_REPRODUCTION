# Historical calibration example

[中文版](README.zh-CN.md)

These files are from 2026-10-16, with a ProGrasp 400093 and that day's ECM and set-up-joint placement.

| File | Meaning |
|---|---|
| calib/manifest.json | Calibration points, reference pose and base position |
| calib/calibration_r6.json | Affine calibration from ECM cp to camera coordinates |
| calib/check2_after_restart2.json | Check record after that day's restart |
| beta/beta_sweep.json | That day's beta measurement |
| camera/ | Camera parameters for both topics |
| calibration_summary.json | Frames, TCP and calibration summary |

Units follow the field names: `*_mm` is millimeters, `*_m` is meters. The physical left camera corresponds to topic right.

This is an **example of the file format and of historical results**. It is not a valid calibration for a new experiment. After any change to the ECM, set-up joints, instrument or placement, recalibrate or check.
