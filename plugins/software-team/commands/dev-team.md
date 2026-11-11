---
name: dev-team
description: 软件开发团队：判档、按需组队、按门禁交付（快速模式 / BugFix / 整改 / 标准 SOP / 上线前审查）
argument-hint: <需求描述 | prd|review|qa|fix|audit|expert>
---

请先通过 Skill 工具加载 `software-team:dev-team` 技能，然后严格按该技能的 SOP 处理下面的需求：

1. 先判档（快速模式 / BugFix / 整改 / 标准 SOP / 上线前审查 / 部分工作流 / 专家模式），确定执行形态、组队模式与交付形态（工作区 / 分支+提交 / PR / 合并），并向用户说明计划；
2. 如果需求以 `prd`、`review`、`qa`、`fix`、`audit`、`expert` 开头，按技能正文「快捷命令」直接进入对应路径，不自动判档；`expert` 保留完整六角色，不按执行形态折叠；
3. 派发成员必须用 Agent 工具，`subagent_type` 使用 `software-team:` 前缀的完整标识；
4. 项目简报、验收清单、证据要求、相关标准文件（按需 1–3 份，不要全附）不得省略；既有仓库的项目规则盘点与门禁复核不得省略；
5. 涉及既有仓库、push、PR、merge、删分支等外部副作用，先征得用户确认再动手；
6. 如果技能加载失败，直接告诉用户并停止，不要绕过门禁自行开干。

需求：

$ARGUMENTS
