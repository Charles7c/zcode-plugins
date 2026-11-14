# zcode-plugins

个人 ZCode 插件库，同时作为**本地插件市场根目录**（把本仓库根目录路径添加进 ZCode 即可）。本仓库是插件源码的唯一真源；ZCode 安装的是缓存副本，改完源码必须「刷新市场 → Update」才生效。

## 目录结构

```
zcode-plugins/
├── README.md
├── marketplace.json                  # 市场目录（市场根级）
└── plugins/
    ├── software-team/                # 软件开发团队
    │   ├── .zcode-plugin/plugin.json
    │   ├── commands/dev-team.md      # 显式入口命令（/dev-team）
    │   ├── skills/dev-team/
    │   │   ├── SKILL.md              # 主理人 SOP（编排入口）
    │   │   └── references/           # 标准库：测试完整性 / 独立复核 / 生产就绪评分卡 / 项目规则盘点 / 验收清单 / 项目简报 / 保障包 / 架构包 / 项目类型 / Bug 诊断
    │   └── agents/                   # 六个成员子智能体
    │       ├── product-manager.md
    │       ├── architect.md
    │       ├── designer.md
    │       ├── engineer.md
    │       ├── reviewer.md
    │       └── qa-engineer.md
    └── qa-team/                      # 质量保障团队
        ├── .zcode-plugin/plugin.json
        ├── commands/test-team.md     # 显式入口命令（/test-team）
        ├── skills/test-team/
        │   ├── SKILL.md              # 主理人 SOP（编排入口）
        │   └── references/           # 测试简报 / 策略 / 用例设计 / 回归选型 / 安全矩阵 / 测试报告
        └── agents/                   # 三个测试成员子智能体
            ├── test-architect.md
            ├── api-test-engineer.md
            └── ui-test-engineer.md
```

## 插件清单

| 插件 | 版本 | 显示名 | 说明 |
|---|---|---|---|
| `software-team` | 1.1.0 | 软件开发团队 | 按风险编排需求、设计、实现、审查与 QA；提供快速模式、BugFix、整改、标准开发和上线前审查；PR 粒度与模板遵循目标仓库规范；依赖/API 升级时评估上游能力与下游简化机会 |
| `qa-team` | 1.1.0 | 质量保障团队 | 编排测试策略、API/UI 执行与独立复核，覆盖接口回归、E2E、安全与发版门禁；按变更选择最小充分回归，依赖/API 替换纳入受影响调用路径验证 |

## 安装与试用

1. ZCode → **插件市场（Plugin Marketplace）** → **Add** → **Add Plugin Marketplace**
2. 粘贴本仓库根目录的本地路径 → 添加
3. **Personal** → `zcode-plugins` → `software-team` → **Install**；需要自动化测试时再安装 `qa-team`
4. 试用：新开会话说「用软件开发团队帮我做一个贪吃蛇小游戏」或直接 `/dev-team 需求...`（命令入口会先加载技能）。
5. 已安装插件的管理入口在 **Settings → Plugins**。

**注意：插件安装的是缓存副本**。改完本仓库源码后，需在插件管理里 **Update**（或卸载重装）才生效。

## 怎么用（用户视角）

安装后**不需要记成员名**，正常提需求即可；主理人自动判档并派发。三种进入方式：

| 方式 | 写法 | 适用 |
|---|---|---|
| 自然语言触发 | 「用软件开发团队做一个贪吃蛇游戏」 | 日常使用，最自然 |
| 命令入口 | `/dev-team 做一个贪吃蛇游戏` | 想确保加载 SOP、或自动触发没命中时 |
| 专家模式 | `/dev-team expert 做一个贪吃蛇游戏` | 需要完整六角色、不做执行形态折叠时 |

### 五种典型用法

1. **快速模式**（单页/小游戏/脚本/单模块小功能）：`做一个 Markdown 编辑器` 或 `给现有模块加一个配置开关`。
2. **标准 SOP**（多模块/前后端/动 API）：`开发一个带登录和订单管理的电商后台`。流程：PRD → 架构与任务 → UI 规范 → 按切片实现 → 审查门禁 → QA → 生产就绪评分。
3. **BugFix**（明确 Bug）：`修复登录后验证码不刷新的问题`。涉及认证/迁移/支付时自动加一道审查。
4. **整改档**（存量 PR/分支/变更）：`把某个 PR 按项目规范修整一下，标题和正文也处理`。
5. **上线前审查**（只读，不写码）：`帮我做一次上线前检查，审查当前分支的改动`。

### 质量保障团队（qa-team）

`qa-team` 可单独使用，也可接 `software-team` 的交付做发版前测试：

1. `/test-team api <repo/commit>`：接口回归，覆盖契约、权限、租户。
2. `/test-team ui <repo/commit>`：UI 冒烟，覆盖关键用户路径。
3. `/test-team security <repo/commit>`：安全基线，认证/越权/租户/注入/上传。
4. `/test-team design <需求>`：只做测试策略与用例设计。
5. 与 dev-team 交接：dev-team 产出 `test-brief.md`，qa-team 回 `test-report.md`；不跨插件直接派子智能体。

主理人宁无漏；成员：测试架构师策周全、接口测试工程师接得稳、UI 测试工程师点得准。

只想要某一环也可以直接说：`只出一份 PRD` / `只做架构评审` / `只做 UI 规范` / `只跑测试`。

### 交付形态

判档时先确定最终落点：工作区改动 / 分支+提交 / PR / 合并。以项目规则和你的明确要求为主；涉及既有仓库、push、PR、merge 的计划会先给你确认再动手。

### 组队模式与产出预算

主理人按任务风险放大队伍，默认小队：

| 模式 | 角色 | 目标产出 |
|---|---|---|
| 单专家 | 1 个专家 | 1 次派发；1 个分析/文档产出 |
| 小队 | 工程师 + QA | 2–3 次派发；代码 + 测试/门禁证据 |
| 加强小队 | 工程师 + QA + 审查者 | 3 次派发；代码 + 审查结论 + 测试证据 |
| 完整队 | PM + 架构师 + 工程师 + QA（+ 设计师/审查者按需） | 4–6 次派发；PRD/设计/代码/审查/QA 证据（按需） |
| 审查队 | 审查者 + QA（+ 保障包） | 2–3 次派发；审查报告 + 门禁证据 |

时间仅作估算，不按墙钟切断；以「是否产出新证据/新文件/新结论」判断进展，连续 2 次无进展或同一错误重复 2 次才停下升级。

**执行形态**：小任务默认单主循环（主理人 + 工程师 + QA），中大型走标准 SOP 的完整角色；新增 subagent 前先过「独立判断 / 并行收益 / 工具隔离」三问。需要保留完整六角色时用 `/dev-team expert <需求>`；qa-team 对应 `/test-team expert <目标>`。

### 使用时会看到什么

- 开场：主理人先说明判档结果、交付形态、要派哪些成员、按什么顺序。
- 过程：成员以子任务形式依次出现；每位交报告后主理人才派下一位，成员之间不直接对话。
- 打断：需求/交付形态不明确、涉及既有仓库或合并需确认、反馈回路 2 轮不收敛、外部服务不可用时，会找你。
- 落盘：`docs/PRD.md`、`docs/system_design.md`、`docs/ui-spec.md`（按档位产生）；交付总结默认只在对话里给。

### 出问题时

| 现象 | 处理 |
|---|---|
| 没触发团队流程 | 改用 `/dev-team 需求`；仍不行则确认插件已启用 |
| 成员没出现/流程被简化 | 检查是否装的是最新版，旧安装需先刷新市场再 Update |
| 报告里出现「未验证」 | 如实标注：该检查没跑成，不是通过 |
| 同一问题 2 轮修不好 | 主理人会停下让你裁决，避免无限循环 |

## 新增插件约定

1. 每个插件放 `plugins/<name>/`，目录名与 `.zcode-plugin/plugin.json` 的 `name` 一致；kebab-case、≤64 字符、初始版本 `0.1.0`。
2. 只声明真实存在的组件（`skills`/`agents`/`commands`/`hooks`/`mcpServers`），路径不得越出插件根。
3. 迭代时改源码并递增语义化版本，同步更新 `marketplace.json` 对应条目的 `version` 与描述。
4. 展示元数据（displayName、i18n、category）放市场条目，插件 manifest 保持功能描述。

## 模型与思考档位

agent frontmatter **不写 `thoughtLevel`**：ZCode 的思考档位只在同时配置 `model` 时才会生效，只写 `thoughtLevel` 会被静默丢弃。默认所有成员继承会话模型；需要按角色指定时，在该 agent 文件中**成对**写入 `model: <providerId>/<modelId>` 与 `thoughtLevel: <low|medium|high|xhigh|max>`。

## 跨 harness 兼容备忘

- 目录布局与 Claude Code 插件格式同构：`skills/<id>/SKILL.md`、`agents/<id>.md`、`commands/*.md`。差异仅在 manifest 位置/名称：ZCode 用 `.zcode-plugin/plugin.json`，Claude Code 用 `.claude-plugin/plugin.json`（字段基本一致），将来加一份镜像即可兼容。
- agents frontmatter 中的 `thoughtLevel`、`injectAgentsMd`、`color` 等扩展字段在非 ZCode harness 需剔除或映射（`tools`/`disallowedTools`/`model`/`description` 大体通用）。
- 本插件的 SOP 刻意只依赖「主智能体 + 一次性子智能体派发（Agent 工具）」，不依赖特定宿主的团队机制，迁移成本低。
- 参考的专家包中 `expertType`/`teamInfo`/`members`（含头像）属于宿主私有元数据，ZCode manifest 不支持，未保留；角色人设已写入各 SKILL/agent 正文。

## 来源

移植自 WorkBuddy 专家包「软件开发团队」（software-company 1.1.0）并吸收多个开源专家团的设计思路，按 ZCode 的子智能体机制重写了协作层。质量靠**确定性规则**（证据关卡、轮次上限、路由判定）而不是堆角色。

方法论参考：[anthropics/single-agent-loop-kit](https://github.com/anthropics/single-agent-loop-kit)（Apache-2.0，reference implementation）；测试完整性规范改写自 UmaDev 知识库（MIT License）；审查清单参考 OWASP/STRIDE。以上均为思路参考，本仓库不复制其代码、不依赖其运行。

## 许可

本项目采用 [Apache License 2.0](LICENSE)。第三方思路来源见上文「来源」段，其各自许可随原项目。
