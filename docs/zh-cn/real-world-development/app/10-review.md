---
title: "第 10 课 - 总结和后续步骤"
description: "回顾 App 工作流程、两个拉取请求（PR）里程碑、画布练习和可复用的质量实践，并探索更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你使用 GitHub Copilot app 完成了 Tailspin Toys 的连续开发流程。你：

- 连接存储库、探索 app 工作区和预置的待办事项，并尝试快速聊天。
- 开始一次有明确目标的星级评分会话，在浏览器画布中审阅结果，并手动合并了第一个拉取请求（PR）。
- 从筛选议题开始，在 **Plan** 模式中确定方案，在 **Autopilot** 模式中实现，并在 **Interactive** 模式中审阅。
- 使用自定义说明引导代理，然后自定义现有的 `quality-checks` 技能并用它运行单元测试、lint 和类型检查。
- 添加 Playwright 模型上下文协议（MCP）服务器，并在真实浏览器中体验筛选功能。
- 创建并选择质量保证（QA）自定义代理，评估需求、覆盖率、技能结果和浏览器证据。
- 审阅完整的筛选更改，并授权 **Agent Merge** 处理第二个 PR。
- 使用现有的 Database Explorer 画布，然后创建并测试由存储库提供数据支持的分流画布。

## 交付的内容

本工作坊包含两个 PR 里程碑，各自基于更新后的 `main` 分支创建独立分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating`，以及明确的未评分状态。
2. **筛选功能及质量工作流**：实现筛选功能，更新指令并将其应用于该功能，自定义 `quality-checks` 报告，创建 QA 配置文件，并包含相关测试。

从规划筛选功能到创建 PR，你一直使用同一个会话、工作树和分支。为了简化工作坊，我们将这些更改合并到了一个 PR 中。之后，你使用了现有的 Database Explorer，并创建了由存储库提供数据支持的分流画布，没有重复执行 PR 工作流程。

## 不同类型的验证

你通过多种方式检查了代码：自动化测试、亲自在浏览器中检查，以及使用 MCP 让 Copilot 操作浏览器。`quality-checks` 技能运行了单元测试、lint 和类型检查，并按新格式报告结果。QA 在 PR 创建前综合检查了这些结果、需求和测试覆盖率。

新增测试应解决实际缺口；如果 QA 检查后发现不需要添加测试，也可能是合理结果。缺少工具、跳过的检查和失败项都是明确的阻碍，不能算作通过。授权合并前，请审阅代码和证据；代码发生更改后，重新检查受影响的证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本工作坊中，你更新了说明、自定义了技能、创建了 QA 配置、配置了 MCP 服务器并创建了画布。可以在不同会话中复用这些自定义项，并根据团队需求调整。说明设定标准，技能描述可重复执行的任务，自定义代理定义专门角色，MCP 服务器连接外部工具，画布提供共享的交互界面。请检查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 跟进重点明确的更改，**仅在任务范围明确且相互独立时**使用 **Autopilot**。日常编辑可选择速度较快的模型；复杂工作则可选择推理能力更强的模型并提高推理强度。

上下文依然与基础设施同样重要。清晰说明要构建*什么*、*为什么*要构建，以及*如何*构建，会显著影响输出。快速聊天适合在开始完整会话前明确想法和范围。

## 更多探索内容

你已经了解核心工作流。以下功能也值得探索：

- [**Automations**][using-automations]：用于重复性或按需任务，例如汇总近期工作。采用前审查计划、权限和范围；创建自动化任务属于后续方向，不是本研讨会的一部分。
- **Rubber duck**：用于讨论问题，并在开始构建前获得有价值的反馈。
- [`/chronicle`][chronicle]：生成会话过程的叙述。
- [Bring your own key (BYOK)][byok]：使用自己提供商的模型，包括通过 Ollama、Foundry Local 或 LM Studio 使用本地模型。
- [Deep links][deep-links]：直接在应用中打开存储库、会话或提示词。

## 后续步骤

持续使用工具是提升熟练度的最佳方式！可以用它处理生产代码、个人项目，或者多年一直想做却尚未着手的小应用。与团队分享经验，也向团队成员学习。别忘了继续探索文档。

如果想继续了解 GitHub Copilot 生态系统，可以查看 [VS Code 学习路径][vscode-harness]、[Copilot CLI 学习路径][cli-harness]或 [Cloud agent 学习路径][cloud-harness]。

## 资源

- [关于 GitHub Copilot app][about-copilot-app]
- [GitHub Copilot app 入门][getting-started]
- [自定义 GitHub Copilot app][customize]
- [使用自动化][using-automations]
- [使用画布扩展][canvas-docs]

[vscode-harness]: ../../vscode/
[cli-harness]: ../../cli/
[cloud-harness]: ../../cloud/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[customize]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
[using-automations]: https://docs.github.com/copilot/how-tos/github-copilot-app/using-automations
[canvas-docs]: https://docs.github.com/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
[chronicle]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/chronicle
[byok]: https://docs.github.com/copilot/how-tos/github-copilot-app/use-byok-models
[deep-links]: https://docs.github.com/copilot/how-tos/github-copilot-app/open-with-deep-links