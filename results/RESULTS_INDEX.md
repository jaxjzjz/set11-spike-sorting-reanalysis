# Set11 新分析：已生成结果清单

英文代码见上一级 set11_analysis.ipynb，中文说明见上一级 set11_report_cn.md。

| 对应步骤 | 文件名 | 内容 | 保存目的 |
|---|---|---|---|
| 参数 | [parameters.csv](parameters.csv) | 本次使用的参数、单位及依据类别 | 区分文件给定值、官方示例与本项目设置 |
| 1 输入检查 | [01_input_check/recording_summary.csv](01_input_check/recording_summary.csv) | 使用通道数、采样率、有效时长、末尾处理理由和时间连续性 | 确定后续使用的有效时间范围 |
| 1 输入检查 | [01_input_check/channels.csv](01_input_check/channels.csv) | 64 通道的位置、连接状态、整段统计和保留决定 | 确定可用通道并提供复核线索 |
| 1 输入检查 | [01_input_check/channel_overview.png](01_input_check/channel_overview.png) | 各通道原始电压波动 | 快速查看通道间幅度差异 |
| 2 预处理 | [02_preprocessing/windows.csv](02_preprocessing/windows.csv) | 六个分层随机窗口的采样点、秒数、种子 | 准确复现本次抽样范围 |
| 2 预处理 | [02_preprocessing/preprocessing_metrics.csv](02_preprocessing/preprocessing_metrics.csv) | 全部通道各窗口各阶段的 MAD 与频带功率 | 量化预处理影响和时间差异 |
| 2 预处理 | [02_preprocessing/window_scale_comparison.csv](02_preprocessing/window_scale_comparison.csv) | 六窗波动相对各通道通常水平的比值汇总 | 检查抽样时间段中的波动稳定性 |
| 2 预处理 | [02_preprocessing/spectra.npz](02_preprocessing/spectra.npz) | 频率、通道、阶段及各通道六窗平均频谱 | 复查每个通道的频谱，不只看总体图 |
| 2 预处理 | [02_preprocessing/preprocessing_overview.png](02_preprocessing/preprocessing_overview.png) | 全通道频谱概览及共同参考前后波动 | 检查预处理在所有通道上的整体影响 |
| 3 正负峰检测 | [03_peak_detection/candidate_peaks.csv](03_peak_detection/candidate_peaks.csv) | 候选峰时间、通道、幅度和方向 | 为相邻接点的事件合并提供输入 |
| 4 跨接点合并 | [04_event_merge/merged_events.csv](04_event_merge/merged_events.csv) | 候选事件时间、代表接点、幅度、方向和组内峰数 | 为多接点波形提取提供事件时间和主接点 |
| 5 波形提取 | [waveforms.npy](05_waveforms/waveforms.npy) | 342,533 个事件的六接点波形 | 用于提取分组特征 |
| 5 波形提取 | [waveform_events.csv](05_waveforms/waveform_events.csv) | 对应事件时间和接点编号 | 对照波形文件中的每一行 |
| 6 初步分组 | [cluster_assignments.csv](06_sorting/cluster_assignments.csv) | 事件编号、主接点和组号 | 用于候选组质量检查 |
| 6 初步分组 | [model_search.csv](06_sorting/model_search.csv) | 各组数的 BIC、收敛状态和搜索上限 | 核对组数选择 |

第1–6步已有结果；第7步已完成全部组的质量指标计算和双相配对检查，尚未赋予质量类别，自动删除关闭。旧的 `waveform_examples.*` 和 `run_info.json` 是历史文件，当前notebook不再生成或使用。

## 本次补充的结果

| 对应步骤 | 文件名 | 内容 | 保存目的 |
|---|---|---|---|
| 3 | [thresholds.csv](03_peak_detection/thresholds.csv) | 各通道噪声尺度、实际阈值、抑制间隔 | 复现找峰条件 |
| 4 | [peak_members.csv](04_event_merge/peak_members.csv) | 每个峰属于哪个事件及其原始属性 | 追查合并与双相配对 |
| 4 | [distance_validation.csv](04_event_merge/distance_validation.csv) | 正式候选峰按距离及极性分开的相关性分位数 | 记录半径30的数据依据 |
| 4 | [correlation_distribution.csv](04_event_merge/correlation_distribution.csv) | 相关性分布的分箱计数 | 查看分布而非只看中位数 |
| 4 | [boundary_check.csv](04_event_merge/boundary_check.csv) | 连续与分块处理的信号和峰比较 | 检查指定边界是否漏检或重复 |
| 4 | [voltage_threshold_comparison.csv](04_event_merge/voltage_threshold_comparison.csv) | 新旧逐通道实际阈值 | 保留只更换阈值的对照输入 |
| 4 | [voltage_threshold_counts.csv](04_event_merge/voltage_threshold_counts.csv) | 相同处理使用新旧阈值的峰数 | 复现阈值对计数的影响 |
| 7 | [biphasic_pairs.csv](07_duplicate_review/biphasic_pairs.csv) | 正负峰编号、所属事件及判断状态 | 记录疑似重复及处理理由 |
| 7 | [unit_labels.csv](07_duplicate_review/unit_labels.csv) | 分组质量标签，目前全部未分类 | 供已确定的分类结果接入，不代替质量检查 |
| 7 | [retained_events.csv](07_duplicate_review/retained_events.csv) | 保留事件、组号及原波形行号 | 后续分析入口；目前未删除任何事件 |

事件编号与原步骤4一致。`retained_events.csv`的`waveform_row`对应原`05_waveforms/waveforms.npy`的行，不能用删减后的新行号直接索引原波形。

## 1 ms间隔对照

| 步骤 | 文件名 | 内容 | 保存目的 |
|---|---|---|---|
| 3对照 | [interval_comparison.csv](03_peak_detection/interval_comparison.csv) | 六窗、全部通道、正负方向的有无间隔峰数 | 复核计数差异 |
| 3对照 | [suppressed_peaks.csv](03_peak_detection/suppressed_peaks.csv) | 舍弃峰、对应保留峰、时间、幅度、中间电压和形状类别 | 逐条追查为什么被舍弃 |
| 3对照 | [suppressed_waveforms.npz](03_peak_detection/suppressed_waveforms.npz) | 每个舍弃峰前后1.5 ms的六接点预处理波形 | 直接检查峰形状；行号对应pair_id |
| 3对照 | [interval_waveform_comparison.png](03_peak_detection/interval_waveform_comparison.png) | 三种越阈情况各一个例子 | 解释1 ms实际舍弃了哪些形状 |
| 3对照 | [interval_plot_examples.csv](03_peak_detection/interval_plot_examples.csv) | 图中三个例子的原始记录 | 复现选图 |

## 第7步质量检查

| 步骤 | 文件名 | 内容 | 保存目的 |
|---|---|---|---|
| 7 | [group_metrics.csv](07_quality/group_metrics.csv) | 全部1,719组的事件数、留一模板相关性、轮廓系数、短间隔和具体问题标记 | 逐组定位分离与波形问题；保留全部计算结果 |
| 7 | [neighbor_pairs.csv](07_quality/neighbor_pairs.csv) | 同主接点最近特征中心配对和模板相关性 | 找到可以直接对照的组号 |
| 7 | [group_overview.png](07_quality/group_overview.png) | 全部候选组的一致性和分离总览 | 查看总体分组质量 |
| 7 | [overlap_example.png](07_quality/overlap_example.png) | CH10-C9与C23的六接点波形、特征、ISI和互相关图 | 展示已定位的分离问题 |

| 7 | [merge_trials.csv](07_quality/merge_trials.csv) | 827对候选组独立试合并前后的分离、波形一致性和短间隔计数 | 定位值得合并复核的3对，记录不采用其他试合并的原因 |
| 7 | [group_review.html](07_quality/group_review.html) | 全部组的可切换波形、ISI和复核表单 | 逐组记录论文要求的波形证据，集中在一个文件中 |

`visual_review.csv`由复核页下载后放入07_quality目录；尚未填写时不生成虚假的人工结论。`group_metrics.csv`新增论文时间条件、证据字段和是否进入第8步的标记。

## 第7步交付文件

| 步骤 | 文件名 | 内容 | 保存目的 |
|---|---|---|---|
| 7 | [unit_summary.csv](07_quality/unit_summary.csv) | 全部组的类别、理由、保留标记及去重前后事件数 | 明确第8步使用哪些单元 |
| 7 | [spike_times.csv](07_quality/spike_times.csv) | 保留单元去重后的事件编号与放电时间 | 作为声音响应分析的输入；当前尚无已审核可用单元 |
