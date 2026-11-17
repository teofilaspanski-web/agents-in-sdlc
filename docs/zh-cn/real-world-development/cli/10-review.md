---
title: "第 10 课 - 回顾和后续步骤"
description: "回顾 Copilot CLI 工作流程、两个拉取请求、可复用的自定义项和后续资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot CLI 完成了 Tailspin Toys 的连续开发流程。你：

- 准备了 Codespace，安装 Copilot CLI，探索项目并找到预置的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 Plan 模式中确定方案，在 Autopilot 模式中实现，并在 Interactive 模式中审阅。
- 使用自定义说明引导代理，然后自定义现有的 `quality-checks` 技能并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义代理，评估需求、覆盖率、技能结果和浏览器证据。
- 审阅完整的筛选更改，并授权 Agent Merge 处理筛选 PR。
- 探索用于上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的成果

本工作坊包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating`，并明确显示未评分状态。
2. **筛选和质量工作流程**：实现筛选、更新说明并将其应用于功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建其 PR，你一直使用同一个对话和分支。为了简化工作坊，我们将这些更改合并到了一个 PR 中。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 操作浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前综合检查了这些结果、需求和测试覆盖率。

新增测试应解决实际缺口；如果 QA 检查后发现不需要添加测试，也可能是合理结果。授权合并前，请审阅代码和证据；代码发生更改后，重新检查受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置并配置了 MCP 服务器。可以在不同对话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复执行的任务，自定义代理定义专门角色，MCP 服务器连接外部工具。请检查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 跟进重点明确的更改，使用 **Autopilot** 处理范围清晰的任务。日常编辑可选择速度较快的模型，复杂工作则可选择能力更强的模型。

上下文依然与基础设施同样重要。清晰说明要构建**什么**、**为什么**要构建，以及**如何**构建，会显著影响输出。

## 继续探索

你已经完成核心工作流程。还可以了解一些其他 CLI 功能：

- 使用 `/review` 请求代码审阅代理分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行协调彼此独立的子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务发送给 Copilot cloud agent。

## 后续步骤

持续使用工具是提升熟练度的最佳方式！可以用它处理生产代码、个人项目，或者多年一直想做却尚未着手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想在终端中扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若想比较其他环境，可以探索 [VS Code 工作坊][vscode]、[GitHub Copilot app 工作坊][app]或 [Copilot cloud agent 工作坊][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
