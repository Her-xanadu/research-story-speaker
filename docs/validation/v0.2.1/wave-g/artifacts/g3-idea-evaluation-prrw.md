# Idea Evaluation — PRRW

## Problem Anchor

当前科学瓶颈是 Story Open Gap 1：低流量 rare-burst 窗口仍会被漏检；这不是增加模块或填充表格，而是当前 WES 机制在一个已命名失败条件下的不足。若 PRRW 成立，需要改变对该失败的解释：端口组合的历史稀有度可能比包大小熵更能提供低流量窗口中的判别信息。该锚点符合 `idea-and-mechanism.md §A`。

## Exact Failure Point

现有机制是 WES：以包大小熵和 log-volume 的混合分数（λ = 0.5）阈值化。必须命中的条件是 MOCK site-A、60 秒窗口、低 packet-volume 的 rare-burst 窗口，且使用同一预先规定的 split；仅在高流量或不同 site/数据 regime 上获胜不算解决该缺口。该条件对应 `idea-and-mechanism.md §B` 以及 `experiment-thinking.md §A–B` 对问题、预测和分析单位的要求。

## Candidate Mechanism

PRRW 在同一 site 的前 12 个窗口上建立 destination-port rarity rank，并以该稀有度信息对当前 60 秒窗口评分，再阈值化作出决定。它不使用 payload、future packets、test-split statistics 或 site-ID；volume 仅作为复用的 listed covariate。候选机制的可检验说法是：在低流量 rare-burst 条件下，端口稀有度信息本身能带来超过机制关闭控制的提升。

## Mechanism Distinction

机制变化位于 `information` 轴：WES 使用 packet-size entropy，而 PRRW 使用相对同 site 最近 K=12 个窗口的 destination-port rarity rank；windowing、threshold 和 volume covariate 属于复用 scaffold。删除 rarity-rank 组件会删除该历史端口信息，方法不再是 PRRW，因而通过 `idea-and-mechanism.md §C` 的 deletion test（§G）。这不是改名、固定权重或额外超参数；K 是信息来源的定义参数，而不是所主张的贡献本身。

## Fatal Flaws

未发现阻断 `ADVANCE` 的致命缺陷（按 `idea-and-mechanism.md §D` 审计）：

- 现有 `DISCOVERY.md` 只说明 port mix 是观察、尚未成为机制，并没有在同一低流量 rare-burst 条件下 refute PRRW。
- 所需历史窗口来自 site 自身已有的前 12 个窗口，不是标签、future packets 或部署不可得的 oracle。
- 方案明确排除 test-split statistics 和 site-ID，未见 obvious leakage。
- 有可观测的事前 falsifier：Full PRRW 相对机制关闭控制的 F1 差值小于 0.08、WES 同步移动、或 rank shuffle 不消除 lift，都会降低对该机制的信念。
- `RESOURCES.md` 授权 one CPU host、single-split、≤4h，作者给出的最小比较在该预算内；50-seed grid 不属于本 gate 的必要部分。

机制关闭控制必须在后续设计中确认确实破坏 rarity order，而不是仅做一个对最终窗口分数不产生变化的排列；这是一项设计核验，不改变当前候选机制的身份判断。

## Closest Work

LITERATURE 中明确的 closest work 是 MOCK-WES-2019：二者都是“window score + threshold”的 decision shape，但机制轴不同。WES 的信息源是 packet-size entropy，PRRW 的信息源是 destination-port rarity rank。当前 LITERATURE 没有记录在此 rare-burst 条件下已有 port-rarity-rank window scorer，因此目前的 novelty threat 是轴相近的窗口阈值 scaffold，而不是同一信息机制；不以新数据集本身作为新颖性依据。

## Rival Explanations

最强竞争解释是：提升来自更合适的阈值/score tuning 或共享 windowing scaffold，而不是端口稀有度这一信息源；在低流量切片上，WES 也可能因当前 λ 或阈值未调好而显得 flat。为削弱该解释，必须在同一 split、同一低流量 rare-burst 条件和等价 windowing/threshold/volume 设置下，同时保留 WES，并比较 Full PRRW 与 mechanism-off。若机制关闭后仍有相同提升，或 WES 同步获得相当提升，就不能把结果归因于 PRRW 的命名机制。

## Discriminating Prediction

在任何运行前固定以下判别：

- Full PRRW F1 − mechanism-off F1 ≥ 0.08，且 WES 在同一 split 上保持 flat 或不产生同等幅度移动：支持“新信息源而非 WES retune”的方向性预测。
- 若 Full PRRW 与 mechanism-off 的差值 < 0.08，降低对 rarity-rank component 做功的信念。
- 若 WES 与 Full PRRW 移动同样多，PRRW 作为不同信息源替代 WES retune 的主张失败。
- 若在窗口内 shuffle ranks 仍保留 lift，则 named mechanism 未被隔离，不能据此支持机制。

这些是 pre-run predictions，不是证据或已完成实验结果；它们提供了会降低信念的 observable outcomes，符合 `scientific-reasoning.md §C–D`。

## Minimum Decisive Experiment

最小决定性比较（不分配 EXP-ID）是：在 MOCK site-A 的低流量 rare-burst 窗口、同一预先固定 split、同一 60 秒 windowing scaffold 和相同 volume covariate 下，运行三路单次 comparison：Full PRRW、保留 scaffold 但将窗口内 rarity ranks 按作者定义置换的 mechanism-off、以及现有 WES。记录三者在目标条件上的 F1，并核验 shuffle 确实破坏 rarity order；不要先做 50-seed grid。

按 `idea-and-mechanism.md §H` 与 `experiment-thinking.md §D–F`，这个 mechanism-off control 隔离端口稀有度组件，WES 是 baseline/closest-work control；结果模式 Full PRRW > mechanism-off 且 WES 不同步提升，才足以改变对候选机制的判断。若该最小比较不能区分上述竞争解释，则不能进入后续 confirmation grid。

## Complexity Budget

`reuse`

- 60 秒 windowing scaffold
- threshold decision shape
- site-local recent-window access
- volume covariate
- one split、CPU-only evaluation

`new`

- destination-port rarity rank against the previous K=12 windows at the same site（唯一主张的新机制）

`excluded`

- packet-size entropy as a PRRW feature
- payload、future-packet 或 test-split information
- site-ID feature
- optional rank-shaped-noise sham
- 50-seed grid、额外数据集、额外模块和未资助的调参扩展

## Recommended Action

ADVANCE
