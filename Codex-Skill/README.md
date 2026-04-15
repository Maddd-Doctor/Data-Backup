# Codex Skill 索引与重建说明

这个目录用于记录 Codex skills 的公开索引、分类说明和重建方法。目标是让一台新设备能够根据公开来源手动复现相同的 skill 环境，同时避免在公开仓库中提交第三方 skill 的完整内容、个人路径或本机私有配置。

## 文件结构

```text
Codex-Skill/
├── README.md
├── skills-inventory.md
├── rebuild-guide.md
└── manifests/
    └── public-skill-index.example.json
```

| 文件 | 用途 |
|---|---|
| `README.md` | 说明本目录的边界、公开分享原则和维护方式 |
| `skills-inventory.md` | 记录已使用 skill 的分类、含义、触发方式和来源 |
| `rebuild-guide.md` | 说明如何在新设备上根据公开来源重新安装或创建 skills |
| `manifests/public-skill-index.example.json` | 给出机器可读索引的示例结构，便于后续自动化 |

## 公开仓库边界

本目录适合公开保存：

| 可以保存 | 不应保存 |
|---|---|
| skill 名称、分类、用途、触发方式 | 第三方 `SKILL.md` 全文 |
| 公开来源 URL、仓库、commit/tag、许可信息 | 第三方脚本、模板、图片、完整资源包 |
| 手动安装或重建步骤 | 凭据、访问令牌、私有配置、本机绝对路径 |
| 对不同 skill 的使用经验和注意事项 | 设备用户名、机器名、非公开地址 |

如果某个 skill 来自第三方项目，公开文档只记录来源和复现方式。是否允许复制、修改或再分发，应以该 skill 原目录中的 license 文件为准。

## Skill 触发方式

Codex skill 的触发主要由 `SKILL.md` 的 YAML front matter 决定，尤其是 `name` 与 `description`。

| 触发方式 | 含义 | 使用场景 |
|---|---|---|
| 顶层自动 | 每次新的顶层对话开始时自动适用，不需要用户点名 | 例如方法论路由、全局工作原则 |
| 强制手动 | 用户明确点名 skill，例如 `$skill-creator`、`skill-creator` 或直接要求使用某个 skill | 用户已经知道需要哪个能力 |
| Agent 自动判断 | 用户没有点名，但任务与某个 skill 的 `description` 明确匹配 | 由 Agent 根据任务上下文判断并加载对应 skill |
| 联动调度 | 一个路由型或流程型 skill 决定是否继续调用其他 skill | 例如组合多个方法或工具完成复杂任务 |

## 目录组织建议

当前 Codex 公开文档说明：一个 skill 是一个包含 `SKILL.md` 的目录，`SKILL.md` 必须包含 `name` 和 `description`。Codex 会从 repository、user、admin、system 等多个位置读取 skills；repository 范围推荐使用 `.agents/skills`，用户范围推荐使用 `$HOME/.agents/skills`，旧版 `$CODEX_HOME/skills` 仍可作为兼容路径使用。

从当前 `openai/codex` 源码看，Codex 会在 skill root 下递归扫描 `SKILL.md`，扫描深度上限为 6，并会跳过以 `.` 开头的普通子目录。因此，分类目录下继续放多个独立 skill 在机制上可以被发现。

实践上更推荐：

| 方案 | 建议 | 说明 |
|---|---|---|
| 多个独立 skill 在 skill root 下并列 | 推荐 | 安装器、文档示例和人工维护都更直观 |
| 在索引文档中把多个 skill 合并为一个大类 | 推荐 | 保留独立触发边界，同时方便理解和分享 |
| 创建一个大 skill，并把子方法放入 `references/` | 适合强绑定子系列 | 适合统一路由，但子方法不再是独立 skill |
| 使用插件分发多个相关 skill | 适合对外打包分发 | 当需要同时分发多个 skill、MCP 或 app 集成时更合适 |
| 仅为了分类移动到深层目录 | 谨慎 | 当前可被发现，但可能不符合安装器和第三方工具的默认假设 |

## 维护原则

新增或调整 skill 记录时，优先更新 [skills-inventory.md](./skills-inventory.md)。如果新增来源或安装方式，再同步更新 [rebuild-guide.md](./rebuild-guide.md) 和 manifest 示例。

公开文档中的路径应使用通用写法，例如：

```text
$HOME/.agents/skills
$CODEX_HOME/skills
.agents/skills
```

不要提交个人用户名、盘符绝对路径、设备名、非公开地址或任何凭据。
