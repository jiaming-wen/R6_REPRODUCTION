# R6 Grasp Reproduction

R6 真机抓针复现材料，包含原始脚本、必要依赖、标定示例，以及成功与失败试验的数据。

该流程使用 FoundationStereo、人工针折线和夹爪模板点选、几何控制完成抓针。FoundationPose 和 RL policy 不参与这版在线抓针流程。

## 开始使用

```bash
git clone https://github.com/jiaming-wen/R6_REPRODUCTION.git
cd R6_REPRODUCTION
```

1. 按 [requirements.md](requirements.zh-CN.md) 检查依赖。
2. 按 [REPRODUCTION_PROTOCOL.md](REPRODUCTION_PROTOCOL.zh-CN.md) 的 A 节复跑随包样例，无需 GPU、ROS 或机器人。
3. 需要真机复现时，完成 GPU 环境核对、当前装置标定和 [SAFETY_CHECKLIST.md](SAFETY_CHECKLIST.zh-CN.md)，再执行协议中的现场步骤。

## 目录

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

## 样例与验证

样例包含图像、人工点选、保存的 FS 视差及执行记录，无需下载全量实验数据即可复跑感知。

| 样例 | 历史抓取结果 | 感知回放与归档抓取点的差值 |
|---|---|---|
| [Q1_C](examples/successful_trial/README.zh-CN.md) | 成功 | 0.00094 mm |
| [T3b_C](examples/failed_trial/README.zh-CN.md) | 失败 | 0.00029 mm |

两例均通过 0.1 mm 的感知回放门槛。感知回放通过不代表真机抓取成功；T3b_C 使用的 T3_C 感知输入也随失败样例提供。

代码、数据和标定文件均从原始材料逐字节复制；新增内容为复现说明与清单。文件来源和 SHA256 见 [COPY_MANIFEST.csv](scripts/required_dependencies/COPY_MANIFEST.csv)。本仓库保留上游 [MIT License](LICENSE)。

GPU 推理与当前真机环境仍待现场验证。历史标定仅作格式和结果参考，不能直接用于新的装置摆放。固定路径和未包含的模型依赖见 requirements.md。

原始流程：[R6 grasp pipeline](https://github.com/jiaming-wen/suturing-policy-sim2real/blob/aae23cf23a9b2a05fb139bcf0ce1524ccff0e5a3/docs/daily/2026-10-16/R6_grasp_pipeline.md)。
