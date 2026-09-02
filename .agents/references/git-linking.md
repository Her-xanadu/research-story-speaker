# Git Linking Reference

V0.1 规范：Experiment 与外部代码仓库、commit、入口、结果之间的绑定与路径恢复。

## 原则

能够恢复科研认知，不代表能够重新获取实验代码、数据和结果。
Cognitive portability does not imply artifact portability.

- 实验代码**必须**使用 Git；workspace Git 与 code Git 是两套独立仓库。
- 代码仓库可在 workspace 内、workspace 外并列、或远程服务器上（见 `RESOURCES.md`）。
- 绑定信息写在 `EXPERIMENTS.md` 对应 section；Codebase 身份写在 `RESOURCES.md`。
- 不要求：一实验一分支、一实验一 commit、一 seed 一 commit、一实验一 tag。保持灵活。

## 每个正式 Experiment 的最小绑定

在 `EXPERIMENTS.md` 中，每个正式 Experiment 至少能定位：

```text
Experiment ID
Codebase ID
Git repository
Git commit
Experiment entry/config
Result location
```

示例：

```markdown
Codebase: detector-main
Git repository: git@github.com:org/project.git
Git commit: abc7812
Entry: experiments/EXP-031/run.py
Results: server-a:/project/results/EXP-031/
```

字段说明：

| 字段 | 说明 |
|------|------|
| **Codebase ID** | `RESOURCES.md` 中登记的资源名 |
| **Git repository** | remote URL 或相对/已知本地路径 |
| **Git commit** | 冻结共享代码版本的 commit hash（可多个，见下） |
| **Entry** | 该实验的入口脚本、配置或最小差异路径 |
| **Results** | 结果目录；可为相对路径、绝对路径或 `host:path` |

## 多 commit 场景

一次 Experiment 中若发现实现问题，可记录修复前后的 commit：

```markdown
Git:
- Initial: abc123
- Fix: def456
Problem: normalization off-by-one invalidated R1
Valid runs: def456
```

不要求为每次 retry 单独建 Experiment；Run 级别记录在 `Runs` 字段。

## 从 workspace 定位代码仓库

当代码位于 workspace 外：

1. 读 `RESOURCES.md`，按 **Codebase ID** 找到条目。
2. 优先使用条目中的 **Preferred relative location**（例如 `../story-research-code`）。
3. 若路径失效，执行**路径恢复五步法**（下节）。
4. 在 `EXPERIMENTS.md` 更新 Git / Entry / Results 绑定（若 commit 或路径变化）。
5. 必要时更新 `RESOURCES.md` 的 **Last known local location**。

不要假设当前工作目录就是代码目录；始终经 RESOURCES → Codebase 定位。

## 路径恢复五步法

`RESOURCES.md` 中记录的路径失效时（换机器、目录移动、clone 位置变化）：

1. **尝试 Git remote** — `git remote -v` / 条目中的 remote URL，重新 clone 或 `git fetch`。
2. **尝试常见相对路径** — 相对 workspace 的 `../<repo-name>`、条目中的 Preferred relative location。
3. **尝试当前环境搜索** — 在已知父目录或 `RESOURCES` 注明的 Remote location 下查找同名仓库。
4. **必要时询问用户** — 仅当以上均失败；说明已尝试的步骤与缺失信息。
5. **更新 RESOURCES** — 定位成功后写回 **Last known local location**，不删除历史 Notes。

路径失效**不**意味着项目状态失效——**仅当**存在稳定代码恢复源时，路径失效才只是定位问题，恢复路径并更新 RESOURCES 即可继续。若代码仓库对象丢失（无 Git remote / mirror / git bundle / archive），则只能恢复科研认知，无法恢复实验实现。

## 推荐代码布局（§15）

若由本框架协助组织代码仓库：

```text
src/                    # 多个实验共享的实现
experiments/EXP-xxx/    # 实验专用入口、配置或最小差异
results/EXP-xxx/        # 该实验的结果
```

规则：

- `src/` 承载共享代码；**Git commit 冻结共享代码版本**。
- `experiments/EXP-xxx/` 只放该实验的入口与配置差异。
- `results/EXP-xxx/` 对应该 EXP-ID 的产物。
- **禁止**为 Experiment ID 复制整个代码库。

## 完整追溯链

任意 `EXP-ID` 应能沿以下链路导航：

```text
EXPERIMENTS.md (EXP-ID section)
  → RESOURCES.md (Codebase ID)
  → code repo (Git commit)
  → Entry (experiments/EXP-xxx/)
  → Runs
  → Results (results/EXP-xxx/)
  → Interpretation / Discovery Impact
  → .research/reviews/EXP-xxx/（若有）
  → REVIEWS.md
```

Gate 验收标准：陌生 Agent 仅凭 workspace 文件即可重建上述链路，无需聊天历史。

## 与 Runs 的关系

- **Experiment** 绑定 Codebase + 主要 commit + Entry + Results 根目录。
- **Run** 可在同一 Experiment 下引用不同 seed、retry 或有效 commit（见 `experiment-record.md`）。
- Results 目录可按 Run 分子目录，但 EXP-ID 级 Results 字段至少指向可发现的总入口。

## Portability

`RESOURCES.md` 中每个 Codebase 记录 **Recovery source** 与 **Portability**。Portability 三值：

- `portable`：至少一种稳定恢复源（Git remote / mirror / git bundle / archive）
- `host-dependent`：例如 local-only，或服务器绝对路径但无 remote
- `unavailable`：当前已无法恢复

**任何将进入 Story Evidence、Reviewer acceptance 或论文级结论的 Experiment，必须具有稳定恢复源。** exploratory 允许 `local-only`，但须标明。
