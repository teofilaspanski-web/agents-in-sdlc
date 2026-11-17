---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确认实施方案，从议题出发用 Autopilot 构建筛选功能，并用 Interactive 模式审阅结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。但较大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流程，帮助我们以合适的方式构建正确的功能。接下来的几课将介绍典型的代理驱动开发流程：从议题着手实现功能、确保代码有效且行为符合预期，最后将更改成功合并到项目中。

> [!NOTE]
> 在整个功能工作流程中，你会继续使用同一个对话和分支。实际工作中，通常会为不同类型的文件创建不同分支或 PR；本练习为了聚焦核心概念而简化了流程。

本课将完成以下目标：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审阅代码。
- 在转发的浏览器中手动验证功能。

继续实现该功能时，你还将更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在明确范围内实施。

## 背景

在开发流程中引入人工智能（AI）编码代理，并不会改变基本原则。事实上，这些原则变得更加重要！大多数开发者的流程大致如下：

1. 从描述待办事项的已创建议题开始。
2. 制定要构建内容的计划。
3. 编写并审阅代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审阅且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准流程，可以确保 AI 生成的代码符合要求，并接受与手写代码相同的审查。

## 对话模式

**对话模式**决定代理拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并反复迭代，直至任务完成。

先使用 Plan 模式，审阅计划，然后使用 Autopilot 实施计划。

## 从议题开始

开始筛选工作前，返回 Codespace，并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd>，打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。使用 `#` 可以将 GitHub 上的议题或拉取请求（PR）加入上下文。

## 规划筛选功能

规划让你有机会在委托 Copilot 实施功能或执行任务前确定实施方案。处理复杂任务时，花些时间规划通常很有帮助。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   为实现此功能制定计划。
   ```

Copilot 会先探索项目，再确定合适的实施方案，并据此制定计划。

3. Copilot 可能会询问筛选功能应如何工作。根据你的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动以审阅计划。
6. 如果计划中的某些内容与你的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

计划已经编写并审阅完毕，现在开始实施。使用 Autopilot 让 Copilot 执行计划。

Autopilot 会让 Copilot 持续迭代问题，直至它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中标签类似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实施，因此请先审阅计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将检查视为已完成。

## 审阅并验证实现

代码生成后，和其他代码一样，必须在合并前进行审阅。现在一起审阅代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 对照议题和规划时确定的内容检查结果。
4. 完成代码审阅后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败项。

## 体验新功能

代码看起来没问题，但运行起来如何？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中尝试筛选功能。告诉我 URL，并让服务器保持运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别、按发行商筛选，以及按计划约定的方式组合筛选条件。
4. 确认重置和无结果时的行为符合议题及你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并审阅了功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审阅代码。
- 在转发的浏览器中手动验证功能。

接下来，使用自定义说明深入了解如何生成代码，并确保代码遵循已记录的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
