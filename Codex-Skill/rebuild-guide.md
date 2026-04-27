# Codex 环境重建指南

本文档说明如何在新设备上根据公开来源重建 Codex 的 skills 与 MCP 环境。默认假设优先由 Agent 执行，人工只在安装软件、重启应用或确认权限时介入。

## 1. Skills 默认位置

Codex 当前公开文档推荐的常见位置：

```text
$HOME/.agents/skills
```

兼容旧环境时，也可能读到：

```text
$CODEX_HOME/skills
```

项目专用 skills 通常放在仓库内：

```text
.agents/skills
```

### 重建策略

| 场景 | 优先位置 | 说明 |
|---|---|---|
| 用户级通用 skills | `$HOME/.agents/skills` | 新设备和跨项目复用优先使用这里 |
| 项目专用 skills | `.agents/skills` | 仅服务当前仓库 |
| 旧环境兼容 | `$CODEX_HOME/skills` | 只作为读取或迁移来源，不作为新安装首选 |

## 2. Skills 重建顺序

1. 安装或更新 Codex，并确认系统内置 skills 已随版本提供。
2. 读取 `skills-inventory.md`，确认需要恢复的 skills。
3. 对公开来源的 skills，优先从原始仓库按指定 repo、path、commit 或 tag 安装。
4. 对自定义 skills，重建同名目录和最小 `SKILL.md` 结构，再补充 `references/`、`scripts/` 或 `assets/`。
5. 安装完成后重启 Codex，让新 skills 被重新扫描。
6. 让 Agent 复核可见 skill 列表，确认名称、触发方式和分类一致。

## 3. MCP 重建顺序

1. 先安装运行时，例如 Python、Node.js 或对应二进制。
2. 按 `mcp-inventory.md` 安装 MCP server 本体。
3. 在 Codex 配置中添加对应 `mcp_servers.<name>` 配置。
4. 如需本地桌面程序配合，先确认该程序已安装并能正常启动。
5. 重启 Codex，使新的 MCP server 被加载。
6. 让 Agent 做一次真实调用验证，而不是只检查配置文件是否存在。

## 4. 需要额外配置的能力

| 名称 | 类型 | 额外依赖 | 重建检查 |
|---|---|---|---|
| `web-access` | skill | Node.js 22+、Chrome remote debugging、CDP proxy 或同类桥接 | 安装后检查浏览器调试端口、登录态风险和本地代理是否正常 |
| `caveman` 系列 | skill | 无强制运行时；可选 Codex plugin 集成 | Codex app 优先使用 skill 版本；支持本地 marketplace 的 IDE 中可再安装 plugin |
| `zotero-mcp` | MCP | Python 3.10+、Zotero 7+、Codex 自定义 MCP 支持 | 检查 Zotero 本体、Local API、数据库可访问性和语义索引状态 |

## 5. `zotero-mcp` 参考配置

### 最小可用配置

直接使用可执行文件：

```toml
[mcp_servers.zotero]
command = "zotero-mcp"

[mcp_servers.zotero.env]
ZOTERO_LOCAL = "true"
PYTHONUTF8 = "1"
```

如果环境里更稳定的是 Python 模块方式，也可以使用：

```toml
[mcp_servers.zotero]
command = "<python>"
args = ["-m", "zotero_mcp"]

[mcp_servers.zotero.env]
ZOTERO_LOCAL = "true"
PYTHONUTF8 = "1"
```

### 语义搜索依赖

```bash
pip install --user "zotero-mcp-server[semantic]"
```

### 首次建立索引

推荐直接建立全文索引：

```bash
zotero-mcp update-db --fulltext
```

需要完全重建时：

```bash
zotero-mcp update-db --force-rebuild --fulltext
```

### `zotero-mcp` 常见注意事项

- 如果只做语义索引更新，优先使用本地数据库路径，稳定性通常高于依赖 Local API 的在线读取。
- 如果要在搜索结果里继续拉取完整 Zotero 条目详情，最好让 Zotero 处于运行状态，并启用 Local API。
- 如果 Zotero 使用自定义数据目录，确保 `zotero-mcp` 进程能解析到真实的 `zotero.sqlite`。必要时应显式配置数据库路径，或使用单独 wrapper 统一运行环境。
- 在部分 Windows 环境中，`localhost` 可能受到代理或 IPv6 解析影响；本地 API 不稳定时，优先检查是否需要改用 `127.0.0.1`。

## 6. 手动创建自定义 Skill

一个 skill 至少需要：

```text
<skill-name>/
└── SKILL.md
```

推荐结构：

```text
<skill-name>/
├── SKILL.md
├── agents/
│   └── openai.yaml
├── scripts/
├── references/
└── assets/
```

`SKILL.md` 需要包含 YAML front matter：

```markdown
---
name: skill-name
description: Clear description of what the skill does and exactly when Codex should use it.
---

# Skill Title

Core workflow and concise instructions.
```

## 7. 提交前隐私检查

公开文档中不要出现：

- 用户名、设备名、绝对本机路径
- 内网地址、私有仓库地址、登录态信息
- token、cookie、API key、会话凭据
- 未确认许可的第三方完整内容
