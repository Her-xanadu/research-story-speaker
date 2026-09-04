# Research Resources

> 从本模板创建 `.research/RESOURCES.md`。记录**资源身份 + 定位提示**，非绝对路径依赖。
> 首次安装时由 `workspace-setup` 填写：**先 `## Compute`（本地或服务器），再 `## Codebases`（Git 与布局 A/B/C）**。

Do not store: passwords, API keys, private tokens, SSH private keys, credentials.
Access hints like `SSH alias gpu-a` are OK.

## Codebases

### {{CODEBASE_ID}}

**Purpose:** {{用途}}

**Git:** {{remote URL}}

**Recovery source:** Git remote | mirror | git bundle | archive | local-only

**Portability:** portable | host-dependent | unavailable

**Preferred relative location:** {{如 ../project-code}}

**Last known local location:** {{绝对路径或 unknown}}

**Remote location:** {{服务器路径或 N/A}}

**Notes:** {{布局模式 A/B/C 等}}

---

## Datasets

### {{DATASET_NAME}}

**Location:** {{路径或 URI}}

**Read/write:** read-only | read-write

**Notes:** {{划分、预处理}}

---

## Compute

### {{COMPUTE_NAME}}

**Use:** {{GPU experiments / CPU / etc.}}

**Access:** {{SSH / local / MCP}}

**Working directory:** {{默认工作目录}}

---

## External Capabilities

| Capability | Available via |
|------------|---------------|
| Literature | Web / PDF / {{Zotero}} |
| Independent reviewer | {{Codex / Claude / MCP}} |
| Git | native |
| Remote compute | {{SSH or equivalent}} |

---

_路径失效时按 git-linking.md 重新定位并更新本文件。_
