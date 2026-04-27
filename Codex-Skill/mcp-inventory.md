# Codex MCP 清单

更新日期：2026-04-27  
记录口径：公开索引，只保留名称、用途、触发方式、依赖、复现方法和公开来源，不记录私有配置或本机专属路径。

## MCP 总览

| 名称 | 类型 | 主要用途 | 首次登记日期 | 开源地址 |
|---|---|---|---|---|
| `zotero-mcp` | `stdio` MCP server | 访问 Zotero 文献库、元数据、笔记、批注、附件、全文检索和语义搜索 | 2026-04-27 | [54yyyu/zotero-mcp](https://github.com/54yyyu/zotero-mcp) |

## `zotero-mcp`

复现方式：优先让 Agent 自动安装并写入 Codex 的 MCP 配置；人工重建时只需要准备 Python、Zotero 和重启权限。

| 项目 | 说明 |
|---|---|
| 含义说明 | 把 Zotero 暴露为 Codex 可调用的文献工具层，覆盖库检索、条目元数据、附件、批注、笔记和语义搜索 |
| 触发方式 | 不属于自动扫描的 skill；Agent 在任务涉及文献库检索、条目读取、笔记/批注处理或语义搜索时主动调用，也可由用户手动点名使用 |
| 运行形态 | MCP server，通常以 `stdio` 方式接入 Codex |
| 基础依赖 | Python 3.10+、Zotero 7+、支持自定义 MCP 的 Codex 客户端 |
| 基础安装 | `pip install --user zotero-mcp-server` |
| 语义搜索依赖 | `pip install --user "zotero-mcp-server[semantic]"` |
| 本地前提 | Zotero 本体可启动；如需通过 Local API 拉取完整条目详情，需在 Zotero 中启用 Local API |
| 索引命令 | `zotero-mcp update-db --fulltext` |
| 重建命令 | `zotero-mcp update-db --force-rebuild --fulltext` |
| 公开复现重点 | 安装 server、写入 Codex 的 `mcp_servers.zotero` 配置、重启 Codex、验证真实调用 |

## 使用说明

### 适合的任务

- 按作者、标题、年份或标签检索 Zotero 条目
- 读取文献元数据、摘要、附件、笔记和批注
- 在大库中进行“按意思找文献”的语义搜索
- 为论文写作、综述整理和证据回查提供文献侧工具支持

### 不同能力的依赖差异

| 能力 | 是否依赖 Zotero 正在运行 | 是否依赖 Local API | 是否依赖语义索引 |
|---|---|---|---|
| 普通条目检索 | 通常建议是 | 常见情况下是 | 否 |
| 附件/批注/笔记读取 | 通常建议是 | 常见情况下是 | 否 |
| 语义搜索召回 | 否，索引可直接基于本地数据库更新 | 否 | 是 |
| 语义搜索后补全完整条目详情 | 建议是 | 是 | 是 |

## 注意事项

- 如果 Zotero 使用自定义数据目录，重建时要确保 `zotero-mcp` 进程能定位真实的 `zotero.sqlite`。
- 首次语义索引可能需要下载 embedding 模型并花费几分钟，后续增量更新会更快。
- 在部分 Windows 环境中，`localhost` 可能受到代理或 IPv6 解析影响；本地 API 不稳定时，优先检查是否需要改用 `127.0.0.1`。
- 公开文档不记录本机绝对路径、实际数据库位置或任何登录信息。
