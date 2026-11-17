---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审阅工作

在本实验中，你使用 GitHub Copilot 完成了多项改善用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包含高对比度和浅色模式切换开关的无障碍功能。现在一起查看代码更改，并在必要时向 Copilot 提供反馈，改进其工作。

### 场景

前面多次提到，引入生成式人工智能（AI）并不会改变软件设计和 DevOps 的基本原则。我们仍然需要审阅生成的代码，并遵循常规 DevOps 流程。因此，现在先审阅 GitHub Copilot 对文档、相关游戏功能和无障碍功能的建议，再让团队其他成员参与审阅。

## GitHub Copilot cloud agent 的安全性

由于 Copilot cloud agent 会在无人监督的情况下异步执行任务，因此设置了一些安全限制，以确保操作安全：

- Copilot 只能读取存储库，并且**只能**写入用于处理代码的分支。
- Cloud agent 在 GitHub Actions 中运行，并创建独立的临时工作环境。
- 运行任何 GitHub Actions 工作流前，都必须获得人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括模型上下文协议（MCP）服务器。

## 审阅生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个拉取请求（PR）：为代码添加文档。这里使用 GitHub.com 标准 PR 界面。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括调用外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot cloud agent 的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成。可能需要几分钟，可以休息片刻或回顾目前学到的内容。

4. 拉取请求准备就绪后，选择 **Files changed** 选项卡并审阅更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包括新生成的 TSDoc 文档注释和其他文档。具体更改因结果而异。

    查看更改时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头。这些来自练习 2 开始时查看的自定义说明文件。

6. 审阅完毕且确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准的指示器。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会出现在拉取请求检查区域的队列中。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 修改

与 Copilot 协作处理拉取请求并非单向过程。你可以像提及团队成员一样，在拉取请求评论或代码内联评论中提及 Copilot。Copilot 会读取这些评论并启动新会话来处理请求。由于结果具有非确定性，无法规定必须使用的提示文字。可以要求 Copilot：

- 在每个代码文件顶部添加简要说明文件用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上述建议，也可以提出其他希望在代码库中看到的文档改进。
2. 选择 **View Session**，观察 Copilot 执行任务。注意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中应会出现新的提交。
5. 选择 **Files changed** 选项卡审阅更改。

可以继续迭代，直至满意。确认满意后，可以将 PR 从草稿转换为准备审阅状态，并合并到 `main` 分支。

![将 PR 转为准备审阅状态](../../../_images/ex7-ready-for-review.png)

## 审阅相关游戏功能

现在返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审阅生成的代码。
5. 审阅完毕并确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流正在等待批准的指示器。
7. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流应会出现在拉取请求检查区域的队列中。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
9. **可选：**也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！和之前一样，可以继续迭代并要求 Copilot 更新。例如，可以要求它调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，尚未更新自定义说明！）。同样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审阅无障碍功能

最后，审阅通过无障碍自定义代理实现的功能。此 PR 应同时包含练习 3 中分配的高对比度模式，以及在 mission control 中于练习 4 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。请等待 Copilot 完成，可能需要几分钟。

4. 选择 **Files changed** 选项卡，审阅生成的代码。
5. 审阅实现，特别关注：
   - 用于切换模式的用户界面（UI）组件。
   - 使用本地存储持久保存用户偏好的方式。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（无障碍富互联网应用（ARIA）标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 审阅完毕并确认更改没有问题后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流正在等待批准的指示器。
8. 如果工作流正在等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流应会出现在拉取请求检查区域的队列中。如果一切正常，单个 Astro 应用的项目检查将通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。前往 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换开关，确认功能正常，并且重新加载页面后设置仍然保留。

请注意，无障碍自定义代理帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像处理前面 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 - 继续委派任务

Cloud agent 很适合处理待办事项，让你可以把注意力转向其他工作。为培养这种工作习惯，可以在存储库中再创建几个议题并分配给 Copilot。例如：

- 在游戏详情页创建支持者意向表单。
- 在游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 学习路径。在这些课程中，你：

- **检查了存储库附带的自定义说明文件**，以便稍后观察它们对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，并观察它设置环境、规划和异步执行任务。
- **创建并使用自定义代理**完成无障碍工作，添加高对比度和浅色模式切换开关。
- **使用 Copilot agents 页面作为任务控制台**，监控并引导无障碍会话。
- **审阅并迭代 cloud agent 的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

你已完成 Cloud agent 学习路径。如果想继续探索，其他学习路径会通过不同方式补充本路径的内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 直接在集成开发环境（IDE）中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中使用 Plan 和 Autopilot 模式、代理技能、QA 自定义代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云端代理桥接，完成两项经审阅的更改。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
