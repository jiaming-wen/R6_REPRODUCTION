# Successful example: Q1_C

[中文版](README.zh-CN.md)

Counted attempt on 2026-10-16, condition C; grasp at the arc midpoint `s=0.5`. Recorded outcome: success.

- `snap0` / `snap1`: left and right images, robot state, FS disparity. Perception can be replayed directly.
- `clicks.json`: the operator's polyline from needle tail to needle tip.
- `perception.json`: the perception result saved on the day, used as the replay reference.
- `template_clicks.json`: the gripper template clicks on the left and right images.
- `trial_state.json`, `trial_log.jsonl`, `preclose_pose.json`: execution state and the pose before closing.
- `pre_close_touch`, `closed`, `lifted_2s`: images before closing, after closing and after lifting.

The success comes from the historical experiment record. A single perception replay only checks that the geometry can be recomputed; it does not reproduce the grasp motion or prove a new real-robot success.

Replay check on 2026-10-20: the script exited normally, the perception checks passed, and the grasp point differed from the archived one by 0.00094 mm (gate 0.1 mm).
