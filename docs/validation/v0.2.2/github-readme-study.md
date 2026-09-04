# GitHub README 与仓库展示调研（v0.2.2）

日期：2026-09-04。  
方法声明：**使用 agent-reach 的 GitHub 平台 / gh 后端**（`agent-reach doctor` → GitHub `active_backend` = `gh CLI`）。

- 星数与元数据：`gh search repos "repo:OWNER/REPO"`；个别仓库 search 不可用时改 `gh api repos/OWNER/REPO`。
- README 正文：`curl -s "https://r.jina.ai/https://raw.githubusercontent.com/OWNER/REPO/HEAD/README.md"`（scikit-learn 为 `README.rst`）。
- 不抄袭文案或 logo。星数为当日快照，仅作 ≥10k 门槛核验。

门槛：下列 12 个仓库均 **stargazers ≥ 10 000**，偏 AI / ML / agent / research tooling。

| # | 仓库 | Stars | 类型 |
|---|------|------:|------|
| 1 | [huggingface/transformers](https://github.com/huggingface/transformers) | 164 781 | ML 框架 |
| 2 | [langchain-ai/langchain](https://github.com/langchain-ai/langchain) | 145 635 | Agent 平台 |
| 3 | [ggml-org/llama.cpp](https://github.com/ggml-org/llama.cpp) | 127 024 | 推理 runtime |
| 4 | [openai/whisper](https://github.com/openai/whisper) | 108 408 | 研究模型 + 代码 |
| 5 | [pytorch/pytorch](https://github.com/pytorch/pytorch) | 102 755 | ML 框架 |
| 6 | [vllm-project/vllm](https://github.com/vllm-project/vllm) | 90 964 | 推理引擎 |
| 7 | [OpenHands/OpenHands](https://github.com/OpenHands/OpenHands) | 86 155 | Coding agent（`gh api`；search 的 All-Hands-AI org 已迁名） |
| 8 | [scikit-learn/scikit-learn](https://github.com/scikit-learn/scikit-learn) | 67 159 | 科研/工程 ML |
| 9 | [microsoft/autogen](https://github.com/microsoft/autogen) | 60 798 | Agent 框架 |
| 10 | [crewAIInc/crewAI](https://github.com/crewAIInc/crewAI) | 58 082 | Multi-agent |
| 11 | [Aider-AI/aider](https://github.com/Aider-AI/aider) | 48 732 | 终端 coding agent |
| 12 | [stanfordnlp/dspy](https://github.com/stanfordnlp/dspy) | 37 766 | 研究导向 LM 编程 |

候选核验但**未计入 12 篇精读**（避免超标或重复）：`jupyter/notebook` 13 332、`continuedev/continue` 35 757、`langchain-ai/langgraph` 41 047、`BerriAI/litellm` 58 023。`ggerganov/whisper.cpp` 已迁到 `ggml-org/whisper.cpp`。

---

## 各仓库学到的手法

### 1. huggingface/transformers

- **首屏**：`<picture>` 深/浅色两套 SVG logo + 居中一句话（pretrained models for inference and training）+ 生态示意图。
- **徽章**：Hub 模型数、CI、License、Docs、Release、CoC、DOI。信息型，不是装饰。
- **采用**：居中头图；release 徽章（静态，因本仓库 private、无 LICENSE、无公开 CI）。深/浅 `picture` 思路——但本仓库头图自带深色底，避免依赖 GitHub 主题切图。
- **不采用**：多语言 README 矩阵（本仓库访客以中文研究者为主，正文中文即可）；CircleCI / Hub endpoint 徽章；第三方 logo 热链。

### 2. langchain-ai/langchain

- **首屏**：深/浅 logo + **一句定位**（The agent engineering platform.）+ 极短 Quickstart（安装 + 三行代码）。
- **克制**：功能列表偏营销（vibrant community 等），与科研工具气质不合。
- **采用**：一句话定位压在头图下；Quick Start 最短真实路径；用 GitHub `TIP`/`CAUTION` 提示边界。
- **不采用**：社交 follow 徽章、下载量、PyPI（本仓库不是 Python 包）。

### 3. ggml-org/llama.cpp

- **头图**：自绘深色 cover SVG（`cover-llama-cpp-dark.svg`），主题无关，始终可读。
- **Quick start**：安装选项并列后立刻给出两条可跑命令。
- **徽章**：License / Release / Nightly；CI 只链真实 workflow。
- **采用**：自绘深色底 SVG banner；Release 用**静态** badge 指向已存在的 `v0.2.2`；功能表克制、用表格。
- **不采用**：Docker/Server CI 徽章（本仓库无 Actions 展示需求，且为 private）；截图热链别人产品。

### 4. openai/whisper

- **首屏**：H1 + 论文/博客/model card 文字链，**没有**营销徽章墙。
- **架构图**：一张 Approach 图，紧挨一段方法说明。
- **诚实**：给出训练时的 Python/PyTorch 版本，同时声明预期兼容范围。
- **采用**：文字导航优于徽章堆砌；架构图紧跟机制说明；诚实写版本与未完成项。
- **不采用**：论文数字/benchmark 表（本框架没有可引用的发表结果）。

### 5. pytorch/pytorch

- **首屏**：`picture` 深浅 logo + **两条高阶能力**（tensor / autograd），然后 TOC。
- **组件表**：库的模块用表格，不写 slogan。
- **采用**：目录；组件/Skills 用表；两句说清「是什么」。
- **不采用**：超长 Installation 树（本仓库没有 pip/conda）；trunk health CI 面板。

### 6. vllm-project/vllm

- **首屏**：深色/浅色 wordmark + 一句产品句 + **文档/论文/社区文字链**（不是 12 个盾牌）。
- **About** 很长，特性子弹偏「清单」。
- **采用**：头图下用 `| Docs | Validation | Release |` 文字链；Getting Started 只保留最短路径。
- **不采用**：把 Gate 证据展开成特性清单；arxiv/Twitter/Slack（无官网、无论文 URL 可写）。

### 7. OpenHands/OpenHands

- **首屏**：logo + 粗定位 + 一行能力边界 + **beta status** 徽章。
- **WARNING** 块写清沙箱/安全边界。
- **采用**：status 徽章写真实状态（private / v0.2.2 / 无 live Gate）；用 `CAUTION` 写诚实边界（compact MISS、根目录 UNINITIALIZED）。
- **不采用**：for-the-badge 彩色墙；产品截图（本仓库无 GUI）；npm 版本。

### 8. scikit-learn/scikit-learn

- **首屏**：CI/License/DOI/PyPI 信息徽章 + 官方 logo + **一句模块定义**（Python module for ML on SciPy）。
- 学术项目：DOI、机构支持、不喊口号。
- **采用**：科研工具语气；不写「赋能/打造/一站式」。
- **不采用**：CI/Codecov/Ruff（private 且无对应 workflow 对外承诺）；DOI（未注册）。

### 9. microsoft/autogen

- **最有价值**：标题旁橙色 **maintenance mode** 徽章 + `CAUTION` 把现状说死，避免访客误判活跃度。
- Quickstart 仍保留，但先声明边界。
- **采用**：把「当前状态」放在首屏附近，而不是埋在文末才坦白；不把旧 Gate 说成新版本能力。
- **不采用**：Discord/LinkedIn/Twitter 社交徽章；引导迁到不存在的「继任产品」。

### 10. crewAIInc/crewAI

- 头图很大，徽章两行（stars/forks/issues + PyPI/Twitter），偏增长黑客。
- 定位句清楚（Crews vs Flows），但后文课程人数等社交证明过满。
- **采用**：用一两对概念对照讲清机制（本仓库：Story vs chat history；workspace vs 代码 Git）。
- **不采用**：stars/forks/issues 盾牌、Trendshift、认证人数、Cloud Trial。

### 11. Aider-AI/aider

- **首屏**：SVG logo + 一句 + **screencast SVG**（自有资产）。
- 徽章含安装量等运营数字；Features 每条一个小图标。
- **采用**：视觉资产必须是本仓库自绘/自渲染；Quick Start 之外再放架构。
- **不采用**：stars、tokens/week、emoji 徽章、热链 aider.chat logo。

### 12. stanfordnlp/dspy

- **首屏**：logo + 斜体定义句（Programming—not prompting）。README **刻意短**，细节推到 docs 站。
- 有论文列表（自己的 arXiv），安装两行。
- **采用**：README 做入口而非论文；Gate 长文链到 `docs/validation/`；一句话把框架差分成「文件即记忆 / 提示词即程序」。
- **不采用**：编造论文列表；把文档站当 homepage（没有独立站点，About 不填 homepage）。

---

## 对本仓库的设计决议

| 手法 | 采用？ | 说明 |
|------|--------|------|
| 深色底自绘 SVG banner | 是 | `docs/assets/banner.svg`，英文以免 GitHub 图片渲染缺 CJK 字体 |
| 架构 SVG + mermaid | 是 | `docs/assets/architecture.svg` + README mermaid；不热链外部图 |
| 一句话定位 | 是 | Markdown 中文一句；banner 英文副标题 |
| 徽章 | 克制 | 静态 release / private / 冻结计数；**无** CI、stars、license（仓库无 LICENSE）、downloads |
| Quick Start 最短路径 | 是 | clone → harness → AGENTS.md → workspace-setup → workspace-resume |
| 目录 | 是 | 短 TOC |
| 特性子弹墙 | 否 | 改为 Skills 表 + 冻结计数表 |
| Status / disclaimer 靠前 | 是 | 根 `.research/` UNINITIALIZED；compact token **MISS**；无 v0.2.2 live Gate |
| 社交/营销徽章 | 否 | 科研工具气质 |
| 独立 homepage URL | 否 | 不编造 |

冻结计数（须与树一致）：canonical 8 · research-loop 1 · subagents 5 · Skills 13 · RI 6 · scripts 0。
