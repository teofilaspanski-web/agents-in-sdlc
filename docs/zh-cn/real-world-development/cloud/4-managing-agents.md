---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，你让 Copilot cloud agent 执行了三项改善用户体验和添加功能的独立任务。云端代理虽然设计为异步自主工作，但监控任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个任务控制台中，可以查看所有带有未关闭拉取请求（PR）的代理任务、探索代理执行的操作，还可以引导正在进行的会话。

本课将完成以下目标：

- 探索代理页面，监控 cloud agent 任务。
- 在会话进行中引导代理添加更多功能。

## 场景

分配高对比度模式任务后，团队意识到也可以顺便添加浅色模式。由于网站样式和切换功能已经在更新，加入这项功能很合理。现在引导代理，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

先查看分配给 Copilot cloud agent 的所有任务的当前状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
   - 从存储库检出代码。
   - 为 Copilot 创建环境。
   - 设置 MCP 服务器。
   - 执行完成所分配任务所需的各种步骤。

> [!NOTE]
> 具体显示的步骤因 Copilot 工作进度和采用的方法而异。

4. 查看右侧显示的拉取请求（PR）面板。你可以在其中查看 PR 和更改的文件，以便进一步监控。

## 引导 cloud agent

了解当前活动任务后，请求 Copilot 在实现高对比度模式的同时加入浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。会话标题因 Copilot 使用的名称和当前工作状态而异。

    ![mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 监控会话几分钟，直到设置完成并开始执行任务。出现与下方示例类似的消息时，表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    在实现高对比度模式的同时，也添加浅色模式。还应提供相应的切换开关，供用户选择所需的显示模式。
    ```

    ![代理页面上突出显示引导活动云端代理任务对话框的屏幕截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 会确认收到提示，并将其纳入工作流程。

## 等待 Copilot 完成工作

和之前一样，Copilot 会继续处理更新后的任务！完成收到消息时正在执行的具体步骤后，它会将新请求加入工作流程。

与之前一样，这需要几分钟。等待时，可以停下来回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，即监控和引导 GitHub Copilot cloud agent 任务的中心位置。通过此任务控制台，可以跟踪所有活动和已完成的任务、审阅进行中的工作，还可以重定向任务并为正在进行的工作提供额外指导。

你了解了：

- 探索 mission control 和代理页面，监控 cloud agent 任务。
- 重定向正在进行的会话，请求添加更多功能。

Copilot 正在完成无障碍功能。接下来，开始[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
