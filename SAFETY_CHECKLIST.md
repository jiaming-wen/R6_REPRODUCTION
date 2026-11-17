# Pre-run checks on the real robot

[中文版](SAFETY_CHECKLIST.zh-CN.md)

These come from the original R6 procedure and the incident record of the same day. Confirm each one before every on-site run.

- [ ] This is a live attempt with images captured in the lab now. No historical images or replay results from `examples/` are sent to the robot.
- [ ] An operator is present, the dVRK is homed, the instrument is confirmed as ProGrasp 400093, and the emergency stop works.
- [ ] The ECM, set-up joints and instrument match the calibration. After any change, the setup has been recalibrated or checked.
- [ ] Both camera topics are publishing. Physical left = topic right. ROS uses SYSTEM_DEFAULT and UDPv4.
- [ ] This run's perception is valid, fresh, and matches this run's start pose and images. It is not a historical replay with a rewritten timestamp.
- [ ] The wrist rotation happens 25 mm above the pad and uses the correct TCP compensation about the tip.
- [ ] Within 20 mm of the pad, each step translates no more than 3 mm.
- [ ] Each phase is run on its own and its output checked. If a phase fails or an image cannot be captured, stop; do not continue to the next phase.
- [ ] Do not repeat descend/touch. Do not take the exit code of `command | tail` as proof that the robot command succeeded.
- [ ] Before closing the jaws, check whether the needle has moved. Abort if it moved more than 1 mm or if this cannot be checked.
- [ ] The 0.15 N insertion-joint force used as the contact criterion on that day has not been independently calibrated. The operator on site decides whether it applies.

The original T3 incident happened because execution continued after an image capture failed, and the tool descended again after contact. Never chain the whole grasp into a single command.
