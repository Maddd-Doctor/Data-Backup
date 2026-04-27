# Codex Skills 与 MCP 索引

本目录用于公开记录 Codex 环境中的可重建能力单元，重点包括：

- `skill` 的分类、用途、触发方式和公开来源
- `MCP` 工具的作用、依赖和重建方法
- 面向 Agent 的统一重建说明

目标是让新设备上的 Agent 优先根据公开来源自动重建环境，同时让人工读者也能快速看懂。公开仓库只保存索引、说明和步骤，不保存第三方完整内容、私有配置或个人信息。

## 文件结构

```text
Codex-Skill/
├── README.md
├── skills-inventory.md
├── mcp-inventory.md
├── rebuild-guide.md
└── manifests/
    └── public-skill-index.example.json
```

| 文件 | 用途 |
|---|---|
| `README.md` | 说明本目录边界、术语区分和维护规则 |
| `skills-inventory.md` | 记录已使用 skills 的分类、用途、触发方式和公开来源 |
| `mcp-inventory.md` | 记录已接入 MCP 的用途、依赖、触发方式和公开来源 |
| `rebuild-guide.md` | 给 Agent 和人工读者的统一重建步骤 |
| `manifests/public-skill-index.example.json` | 机器可读索引示例 |

## 术语区分

| 类型 | 定义 | 常见形式 |
|---|---|---|
| `skill` | Codex 在合适场景下调用的一组指令 | 一个包含 `SKILL.md` 的目录 |
| `plugin` | 打包分发的一组能力，可能同时包含多个 skills、MCP 或其他集成 | `.codex-plugin/plugin.json` |
| `MCP` | 供 Codex 通过协议调用的外部工具服务 | `stdio` / `streamable-http` server |

## 公开仓库边界

| 可以保存 | 不应保存 |
|---|---|
| skill 或 MCP 名称、分类、用途、触发方式 | 第三方 `SKILL.md` 全文或插件完整内容 |
| 公开来源 URL、仓库、版本、许可信息 | 私有脚本、完整资源包、内部配置 |
| 手动或 Agent 可执行的重建步骤 | 用户名、设备名、绝对本机路径、内网地址 |
| 使用经验、注意事项、兼容性说明 | token、cookie、API key、登录态 |

## 维护规则

1. 新增 skill 时，更新 `skills-inventory.md`；如果安装方式或运行前提特殊，同时更新 `rebuild-guide.md`。
2. 新增 MCP 时，更新 `mcp-inventory.md`；如果需要额外依赖、索引、启动顺序或验证步骤，同时更新 `rebuild-guide.md`。
3. 文档中只使用通用路径写法，例如 `$HOME/.agents/skills`、`.agents/skills`、`$HOME/.config/...`，不要记录本机专属路径。
4. 公开文档只写“别人或 Agent 如何复现”，不要写“当前机器上的隐私细节”。

## 推荐阅读顺序

1. 先看 `README.md`，理解公开仓库边界和术语。
2. 再看 `skills-inventory.md` 和 `mcp-inventory.md`，确认需要重建哪些能力。
3. 最后看 `rebuild-guide.md`，按统一流程恢复。
