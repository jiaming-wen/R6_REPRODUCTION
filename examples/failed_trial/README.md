# Failed example: T3b_C

[中文版](README.zh-CN.md)

Counted attempt on 2026-10-16, condition C; grasp toward the needle tail at `s=0.25`. Recorded outcome: failure (no valid contact; the jaw tips landed inside the arc).

This attempt reused the perception from T3_C, taken with the same placement, so this attempt's `snap0` has no FS disparity and there is no `snap1`. **This is how the original record was taken, not a packaging omission.**

- `snap0`: T3b_C's own start image and state.
- `perception_reference/snap0`, `snap1`: the reused T3_C images and FS disparity, for replay.
- `clicks.json`, `perception.json`: the reused clicks and the perception saved on the day.
- `trial_state.json`, `trial_log.jsonl`, `preclose_pose.json` and phase images: this attempt's execution record.

The protocol replays the failed example from the two frames in `perception_reference`. Perception may pass its checks; **passing perception does not mean the grasp succeeds**. This failure is included to show the difference.

Replay check on 2026-10-20: the script exited normally, the perception checks passed, and the grasp point differed from the archived one by 0.00029 mm (gate 0.1 mm); the historical grasp outcome remains a failure.
