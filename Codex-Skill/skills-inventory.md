# Codex Skill 清单

更新日期：2026-04-15
记录口径：公开索引，仅保留名称、分类、含义、触发方式和开源地址；复现方式按大类统一说明。

## 大类总览

| 大类 | 包含 skill | 用途 |
|---|---|---|
| 求是 / 思想武器 skill | `arming-thought`、`investigation-first`、`contradiction-analysis`、`practice-cognition`、`mass-line`、`criticism-self-criticism`、`protracted-strategy`、`concentrate-forces`、`spark-prairie-fire`、`overall-planning`、`workflows` | 决策、调查、取舍、复盘、长期推进等通用方法论 |
| 内容与媒介生成 skill | `slides`、`research-paper-writing`、`imagegen` | 制作演示文稿、改进学术论文、生成或编辑位图图像 |
| OpenAI / Codex 平台 skill | `openai-docs`、`plugin-creator`、`skill-creator`、`skill-installer` | 查询 OpenAI 官方文档、创建插件、创建或安装 skill |

## 求是 / 思想武器 Skill

这一组 skill 属于同一套方法论子系列。文档中合并为“求是 / 思想武器 skill”大类，但每个 skill 仍保持独立触发边界。使用原则是：不为了形式完整而全部加载，只在任务明显匹配、能提高判断质量或遇到阻塞时调用。

复现方式：让 Agent 按本表链接从 [HughYau/qiushi-skill](https://github.com/HughYau/qiushi-skill) 安装 `skills/` 下对应目录；手动操作时复制同名 skill 目录到目标 skills 路径即可。

| Skill | 含义说明 | 触发方式 | 开源地址 |
|---|---|---|---|
| `arming-thought` | 顶层方法论路由器，先确立“实事求是”：先看事实，再下判断；必要时选择下游思想武器 | 顶层自动；也可强制手动 | [skills/arming-thought](https://github.com/HughYau/qiushi-skill/tree/main/skills/arming-thought) |
| `investigation-first` | 调查研究：在事实、上下文、一手信息不足时，先调查再建议 | Agent 自动判断；强制手动 | [skills/investigation-first](https://github.com/HughYau/qiushi-skill/tree/main/skills/investigation-first) |
| `contradiction-analysis` | 矛盾分析：识别主要矛盾、次要矛盾和约束关系，找到切入口 | Agent 自动判断；强制手动 | [skills/contradiction-analysis](https://github.com/HughYau/qiushi-skill/tree/main/skills/contradiction-analysis) |
| `practice-cognition` | 实践认识论：把方案、假设或判断投入实践，再通过反馈迭代 | Agent 自动判断；强制手动 | [skills/practice-cognition](https://github.com/HughYau/qiushi-skill/tree/main/skills/practice-cognition) |
| `mass-line` | 群众路线：收集多方意见，综合成方案，再回到真实用户或执行者中验证 | Agent 自动判断；强制手动 | [skills/mass-line](https://github.com/HughYau/qiushi-skill/tree/main/skills/mass-line) |
| `criticism-self-criticism` | 批评与自我批评：完成后进行质量审视、处理反馈、纠偏复盘 | Agent 自动判断；强制手动 | [skills/criticism-self-criticism](https://github.com/HughYau/qiushi-skill/tree/main/skills/criticism-self-criticism) |
| `protracted-strategy` | 持久战略：长期复杂目标分阶段推进，通过小胜积累整体胜利 | Agent 自动判断；强制手动 | [skills/protracted-strategy](https://github.com/HughYau/qiushi-skill/tree/main/skills/protracted-strategy) |
| `concentrate-forces` | 集中兵力：当时间、注意力、算力或预算分散时，确定主攻方向 | Agent 自动判断；强制手动 | [skills/concentrate-forces](https://github.com/HughYau/qiushi-skill/tree/main/skills/concentrate-forces) |
| `spark-prairie-fire` | 星火燎原：从零起步时找到最小可行切入口，先建立稳定根据地 | Agent 自动判断；强制手动 | [skills/spark-prairie-fire](https://github.com/HughYau/qiushi-skill/tree/main/skills/spark-prairie-fire) |
| `overall-planning` | 统筹兼顾：在多个目标、利益方和系统约束之间动态平衡 | Agent 自动判断；强制手动 | [skills/overall-planning](https://github.com/HughYau/qiushi-skill/tree/main/skills/overall-planning) |
| `workflows` | 工作流组合：当任务需要多个思想武器串联时，选择标准化跨 skill 流程 | Agent 自动判断；强制手动；联动调度 | [skills/workflows](https://github.com/HughYau/qiushi-skill/tree/main/skills/workflows) |

## 内容与媒介生成 Skill

复现方式：让 Agent 按开源地址安装对应目录；`openai/skills` 来源优先使用 `skill-installer`，其他来源复制表中目录即可。

| Skill | 含义说明 | 触发方式 | 开源地址 |
|---|---|---|---|
| `slides` | 创建或编辑 `.pptx` 演示文稿，使用 PptxGenJS、布局工具和渲染验证 | Agent 自动判断；强制手动 | [openai/skills: skills/.curated/slides](https://github.com/openai/skills/tree/main/skills/.curated/slides) |
| `research-paper-writing` | 改进 ML/CV/NLP 风格学术论文的结构、段落流、claim-support 对齐和审稿呈现 | Agent 自动判断；强制手动 | [Master-cai/Research-Paper-Writing-Skills: research-paper-writing](https://github.com/Master-cai/Research-Paper-Writing-Skills/tree/main/research-paper-writing) |
| `imagegen` | 生成或编辑位图图像，例如照片、插画、纹理、sprite、mockup、透明背景 cutout | Agent 自动判断；强制手动 | [openai/skills: skills/.curated/imagegen](https://github.com/openai/skills/tree/main/skills/.curated/imagegen) |

## OpenAI / Codex 平台 Skill

复现方式：这些通常随 Codex 安装提供。表中的开源地址用于核对内容或在特定环境中手动恢复。

| Skill | 含义说明 | 触发方式 | 开源地址 |
|---|---|---|---|
| `openai-docs` | 构建 OpenAI 产品或 API 时查询最新官方文档，包含模型选择、升级和提示词迁移指导 | Agent 自动判断；强制手动 | [openai/codex: samples/openai-docs](https://github.com/openai/codex/tree/main/codex-rs/skills/src/assets/samples/openai-docs) |
| `plugin-creator` | 创建 Codex plugin 目录、`.codex-plugin/plugin.json` 和可选插件结构 | Agent 自动判断；强制手动 | [openai/codex: samples/plugin-creator](https://github.com/openai/codex/tree/main/codex-rs/skills/src/assets/samples/plugin-creator) |
| `skill-creator` | 创建或更新 Codex skill 的指导 skill，定义结构、front matter、资源组织和验证流程 | Agent 自动判断；强制手动 | [openai/codex: samples/skill-creator](https://github.com/openai/codex/tree/main/codex-rs/skills/src/assets/samples/skill-creator) |
| `skill-installer` | 从 curated list 或 GitHub repo/path 安装 Codex skill | Agent 自动判断；强制手动 | [openai/codex: samples/skill-installer](https://github.com/openai/codex/tree/main/codex-rs/skills/src/assets/samples/skill-installer) |

## 后续登记模板

新增 skill 时，按下面字段补充到对应大类：

| 字段 | 填写方式 |
|---|---|
| Skill | 使用文件夹名和 front matter `name` |
| 含义说明 | 用一句话说明能力边界，不复制大段原文 |
| 触发方式 | 标注“强制手动”“Agent 自动判断”“顶层自动”或“联动调度” |
| 开源地址 | 公开仓库 URL；如果来自官方内置 skill，优先记录可公开查看的源码目录 |
| 复现方式 | 若同一大类来源一致，在大类说明中统一描述；不要在每行重复相同安装句式 |
