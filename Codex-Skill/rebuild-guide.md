# Codex Skill 重建指南

本文档说明如何在新设备上根据公开来源重建 Codex skill 环境。这里不包含第三方 skill 的完整内容，只记录可复现流程。

## 目标位置与迁移策略

Codex 当前公开文档推荐的常见位置：

```text
$HOME/.agents/skills
```

兼容旧环境或既有安装时，也可能使用：

```text
$CODEX_HOME/skills
```

repository 级 skills 通常放在项目内：

```text
.agents/skills
```

如果不确定使用哪个位置，优先查看当前 Codex 版本的官方文档和本机配置。

重建设备时，优先同步到当时 Codex 官方推荐的位置。当前推荐策略是：

| 场景 | 优先位置 | 说明 |
|---|---|---|
| 用户级通用 skills | `$HOME/.agents/skills` | 新安装和跨项目复用优先使用这里 |
| 项目专用 skills | `.agents/skills` | 只服务当前仓库的 skill 放在项目内 |
| 旧环境兼容 | `$CODEX_HOME/skills` | 仅作为已有 skill 的读取或迁移来源，不作为新安装首选 |

如果旧目录中已有 skill，让 Agent 先列出新旧目录清单并按名称、来源、版本对比，再决定重新从开源地址安装、迁移，或保留兼容副本。不要直接覆盖已有目录。

## 推荐重建顺序

1. 安装或更新 Codex，确认 `.system` skills 已随 Codex 提供。
2. 根据 [skills-inventory.md](./skills-inventory.md) 逐项确认需要的 skills。
3. 对公开来源的 skills，优先从原始来源按指定 repo、path、commit 或 tag 安装。
4. 对自定义 skills，使用 `skill-creator` 创建同名 skill，再补充必要的 `SKILL.md`、`references/`、`scripts/` 或 `assets/`。
5. 安装完成后重启 Codex，确保新的 skills 被加载。
6. 让 Codex 读取可用 skill 列表，确认名称、触发说明和分类与清单一致。

## 需要额外配置的 Skill

部分 skill 不是单纯复制目录即可完整启用，还依赖本地运行环境。重建时应在安装后执行前置检查，并把风险提示展示给用户。

| Skill | 额外依赖 | 重建检查 |
|---|---|---|
| `web-access` | Node.js 22+、Chrome remote debugging、CDP Proxy、本地浏览器权限 | 安装后运行 skill 自带依赖检查脚本；确认 Chrome 已允许 remote debugging；使用真实浏览器登录态前提醒账号风控风险 |

`web-access` 的完整浏览器模式会连接本机 Chrome，并可能使用已有登录态操作网页。只在可信设备和可信 Agent 环境中启用，不要把 CDP 调试端口暴露给不可信网络。

## 使用 `skill-installer`

当 skill 来自公开 GitHub 仓库时，可以使用 Codex 预装的 `skill-installer`，或参考其安装逻辑。

示例：

```bash
scripts/install-skill-from-github.py --repo openai/skills --path skills/.curated/<skill-name>
```

如果需要指定版本：

```bash
scripts/install-skill-from-github.py --repo <owner>/<repo> --ref <commit-or-tag> --path <path-to-skill>
```

安装后通常需要重启 Codex 才能识别新 skill。

## 手动创建自定义 Skill

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

`SKILL.md` 必须包含 YAML front matter：

```markdown
---
name: skill-name
description: Clear description of what the skill does and exactly when Codex should use it.
---

# Skill Title

Core workflow and concise instructions.
```

关键要求：

| 项目 | 要求 |
|---|---|
| `name` | 与文件夹名一致；只使用小写字母、数字和连字符 |
| `description` | 说明 skill 做什么、什么时候应该被触发 |
| 正文 | 只写触发后才需要的核心流程 |
| `references/` | 放详细背景、长文档、规范或 schema |
| `scripts/` | 放稳定、重复、容易出错的自动化步骤 |
| `assets/` | 放模板、图片、字体、样例文件等输出资源 |

## 目录扫描规则

一个 skill 是一个包含 `SKILL.md` 的目录。当前 Codex 源码会在 skill root 下递归扫描 `SKILL.md`，但为了兼容安装器、文档示例和人工维护，建议优先采用扁平结构：

```text
skills/
├── skill-a/
│   └── SKILL.md
└── skill-b/
    └── SKILL.md
```

如果一组方法高度绑定，也可以做成一个大 skill：

```text
skills/
└── qiushi-methods/
    ├── SKILL.md
    └── references/
        ├── investigation-first.md
        └── contradiction-analysis.md
```

## 公开清单维护

每次新增 skill 后，更新：

| 文件 | 需要更新的内容 |
|---|---|
| `skills-inventory.md` | 分类、含义、触发方式、来源、复现提示 |
| `rebuild-guide.md` | 如果引入新的安装方式或位置，补充步骤 |
| `manifests/public-skill-index.example.json` | 如果需要机器可读索引，补充示例字段 |

## 隐私检查

提交前检查文档中不要包含：

| 类型 | 示例 |
|---|---|
| 个人路径 | 用户名、盘符绝对路径、主目录绝对路径 |
| 非公开地址 | 内网地址、需要授权访问的 URL |
| 凭据 | 访问令牌、会话凭据、登录凭据 |
| 设备信息 | 机器名、安装 ID、本地会话路径 |
| 第三方完整内容 | 未确认许可的完整 `SKILL.md`、脚本、模板或资源 |

可使用类似命令做文本扫描：

```powershell
$path = 'Codex-Skill'
$patterns = @('<local-user-name>', '<absolute-local-path>', '<login-placeholder>')

foreach ($pattern in $patterns) {
  Select-String -Path "$path/**/*.md", "$path/**/*.json" -Pattern $pattern -SimpleMatch
}
```
