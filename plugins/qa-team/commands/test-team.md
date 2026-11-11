---
name: test-team
description: 质量保障团队：按测试战役编排测试架构师/接口测试工程师/UI 测试工程师，产出测试简报、用例、证据报告与放行/阻塞结论（接口回归 / E2E / 安全基线 / 发版前门禁）
argument-hint: <测试目标 | api|ui|security|design|review|expert> <仓库/commit/环境>
---

请先通过 Skill 工具加载 `qa-team:test-team` 技能，然后严格按该技能的 SOP 处理下面的测试任务：

1. 先判档（全量战役 / 接口回归 / UI 冒烟 / 安全基线 / 只做设计 / 报告评审 / 专家模式），确定执行形态、范围、环境、数据与退出标准；`expert` 保留 test-architect 的完整设计与复核，不按轻量路径折叠；
2. 输入优先读 `test-brief.md`；缺失时一次问清仓库、commit、环境、验收清单、风险区域（≤3 个问题）；
3. 派发成员必须用 Agent 工具，`subagent_type` 使用 `qa-team:` 前缀的完整标识；
4. 测试用例、执行证据、测试报告不得省略；未测项与残余风险必须单列；
5. 测试成员只改测试资产，不改产品代码、不提交、不推送、不合并；
6. 与 `software-team` 协作时只通过 `test-brief.md` / `test-report.md` 交接，不跨插件直接派子智能体。

测试任务：

$ARGUMENTS
