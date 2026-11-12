# Set11 新分析：已生成结果清单

[Notebook](../set11_analysis.ipynb) · [中文报告](../README.md)

| 对应步骤 | 文件名 | 内容 | 保存目的 |
|---|---|---|---|
| 参数 | [parameters.csv](parameters.csv) | 本次使用的参数、单位及依据类别 | 区分文件给定值、官方示例与本项目设置 |
| 1 输入检查 | [01_input_check/recording_summary.csv](01_input_check/recording_summary.csv) | 使用通道数、采样率、有效时长、末尾处理理由和时间连续性 | 确定后续使用的有效时间范围 |
| 1 输入检查 | [01_input_check/channels.csv](01_input_check/channels.csv) | 64 通道的位置、连接状态、整段统计和保留决定 | 确定可用通道并提供复核线索 |
| 1 输入检查 | [01_input_check/channel_overview.png](01_input_check/channel_overview.png) | 原始电压波动和探针接点分布 | 快速查看信号差异与位置关系 |
| 2 预处理 | [02_preprocessing/windows.csv](02_preprocessing/windows.csv) | 六个分层随机窗口的采样点、秒数、种子 | 准确复现本次抽样范围 |
| 2 预处理 | [02_preprocessing/preprocessing_metrics.csv](02_preprocessing/preprocessing_metrics.csv) | 全部通道各窗口各阶段的 MAD 与频带功率 | 量化预处理影响和时间差异 |
| 2 预处理 | [02_preprocessing/window_scale_comparison.csv](02_preprocessing/window_scale_comparison.csv) | 六窗波动相对各通道通常水平的比值汇总 | 检查抽样时间段中的波动稳定性 |
| 2 预处理 | [02_preprocessing/spectra.npz](02_preprocessing/spectra.npz) | 频率、通道、阶段及各通道六窗平均频谱 | 复查每个通道的频谱，不只看总体图 |
| 2 预处理 | [02_preprocessing/preprocessing_overview.png](02_preprocessing/preprocessing_overview.png) | 全通道频谱概览及共同参考前后波动 | 检查预处理在所有通道上的整体影响 |
| 3 正负峰检测 | [03_peak_detection/candidate_peaks.csv](03_peak_detection/candidate_peaks.csv) | 候选峰时间、通道、幅度和方向 | 为相邻接点的事件合并提供输入 |
| 4 跨接点合并 | [04_event_merge/merged_events.csv](04_event_merge/merged_events.csv) | 候选事件时间、代表接点、幅度、方向和组内峰数 | 为多接点波形提取提供事件时间和主接点 |

第 1 步和第 2 步的抽样检查已有结果。第 3 步全程检测和第 4 步跨接点合并已完成。
