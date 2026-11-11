# AGENTS.md

## 项目定位

个人 ZCode 插件库，同时作为**本地插件市场根目录**（把本目录路径添加进 ZCode 即可）。本仓库是插件源码的唯一真源；ZCode 安装的是缓存副本，改完源码必须「刷新市场 → Update」才生效。

## 目录结构

```
marketplace.json                  # 市场目录：插件条目、展示元数据
plugins/<name>/                   # 每个插件一个目录，目录名 = manifest name
  .zcode-plugin/plugin.json       # 插件清单：name/version/description + 组件目录
  skills/<id>/SKILL.md            # 技能（模型读的流程定义）
  skills/<id>/references/         # 技能引用的标准文档
  agents/<id>.md                  # 子智能体定义
  commands/<id>.md                # 斜杠命令入口
  hooks/hooks.json                # 钩子（如需要）
```

当前插件：`software-team`（软件开发团队，显示名「软件开发团队」）、`qa-team`（质量保障团队，显示名「质量保障团队」）。

## 开发约定

1. 目录名与 `plugin.json` 的 `name` 必须一致；kebab-case、≤64 字符；版本遵循语义化版本。
2. 迭代同时递增 `plugin.json` 与 `marketplace.json` 对应条目的 `version`，并同步描述。
3. 只声明真实存在的组件目录（`skills`/`agents`/`commands`/`hooks`/`mcpServers`），路径不得越出插件根。
4. 展示元数据（`displayName_i18n`、`description_i18n`、`category`）放市场条目；i18n 键用 `en` 与 `zh-CN`，不要用 `en-US`。
5. agent frontmatter 中 `thoughtLevel` **必须与 `model` 成对出现**，否则会被运行时静默丢弃；不指定模型就两者都不写。
6. 只读角色用 `disallowedTools: ["Write", "Edit", "ApplyPatch"]`；但 Bash 仍可改文件，只读是「半硬」约束，需在正文写明边界。
7. 技能正文引用标准文件用 `${ZCODE_SKILL_DIR}/references/...`（加载时展开为绝对路径）；派发子智能体时由主理人传绝对路径，子智能体不加载父技能。
8. 主理人的编排只依赖「主智能体 + 一次性子智能体派发」，不引入 TeamCreate/SendMessage 等宿主私有机制。
9. 文档、注释、提交信息用中文；专业术语（API、SDK、PR）保留英文。
10. 新增能力优先做成 reference/tool，不新增 agent；新增 agent 前必须过「派发三问」并说明命中项；角色总数上限 9。

## 提交前校验

1. `marketplace.json` 与 `plugin.json` 可被 JSON 解析。
2. 市场规模条目的 `source` 目录名、`name`、`version` 与插件 manifest 一致。
3. `skills`/`agents`/`commands` 目录真实存在且位于插件根内。
4. 每个 `SKILL.md` 与 `agents/*.md` 有 `name` + `description` frontmatter；命令文件有 `description` 且含 `$ARGUMENTS`。
5. 无未解决占位符：TODO、示例插件名，以及形如双花括号包裹的模板变量，均须在提交前替换或移除。
6. 插件正文不得引用仓库外的本地绝对路径；派发时由主理人传运行时展开的绝对路径。

## 提交约定

中文 Conventional Commits，例如：

```
feat: 新增 software-team 插件 0.3.0
fix: 修正命令入口的技能引用
docs: 补充安装与使用说明
```

## 分支策略

**只在 `main` 上直接提交**：不建功能分支、不做分支合并、不产生 merge commit，保持线性历史。需要试验时用临时文件或 stash 验证，确认后再提交到 `main`。
