---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查更改，并通过第一个拉取请求合并代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行第一次更改了。先从小处着手：游戏数据中已经有星级评分，但首页的游戏卡片尚未显示评分。让代理显示评分、审阅更改，并通过第一个拉取请求将其合并。

本课将完成以下目标：

- 在功能分支上开始一次有明确目标的 Copilot 对话。
- 请求代理对项目进行小幅更改。
- 使用 `/diff` 审阅更改。
- 运行应用，并在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示该评分。不过，首页上的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，让代理在每张卡片上显示现有评分。这项更改小而独立，非常适合作为第一次练习。

## 对话的组成

**对话**是使用 Copilot CLI 完成任务的工作区。与 Copilot app 不同，普通的 CLI 对话使用终端当前检出的存储库和 Git 分支，而不会创建专用工作树。保存的对话可以让你稍后继续讨论；文件和分支仍是磁盘上的普通 Git 状态。

对话中有三类内容：你输入的提示和代理的回复、代理探索和编辑文件时使用的工具，以及可通过 `/diff` 检查的更改。

## 开始对话并提出更改请求

先开始一次新对话，以便着手实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保开始新会话。
5. 使用以下提示请求更改：

   ```plaintext
   在列表页的游戏卡片上显示每款游戏的 starRating（满分 5 星）。如果评分为 null，则显示 "No rating yet"。保持卡片布局不变，添加测试并运行相关检查。
   ```

Copilot 会探索项目，找到显示游戏详情所用的文件，并创建所需代码。现在，你已使用 Copilot CLI 添加了新功能！

## 审阅差异

合并前必须审阅所有人工智能（AI）生成的更改，即使更改很小也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个已更改的文件。
2. 确认游戏卡片在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 检查 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 完成审阅后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式人工智能工具一样，Copilot 的结果具有概率性，并非完全可预测，因此实际代码可能有所不同。请检查行为，不要期待某种特定实现。

## 检查更改

不能只阅读代码就假定它能正常工作。让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的用户界面（UI）。

1. 请求 Copilot 启动应用：

   ```plaintext
   启动应用，让我在浏览器中检查星级评分的更改。告诉我 URL，并让服务器保持运行。
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示的评分满分为五颗星。
4. 返回 Copilot，并要求它停止启动的服务器：

   ```plaintext
   停止你启动的开发服务器。
   ```

## 创建并合并第一个拉取请求

功能已经完成！接下来创建拉取请求（PR），将新代码合并到项目中。

1. 请求默认代理提交更改：

   ```plaintext
   使用合适的提交消息提交已审阅的星级评分更改。
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，打开 PR。
4. 检查更改的文件和检查结果。
5. 准备就绪后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，输入 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot CLI 发布了第一个更改。具体来说，你：

- 在功能分支上开始了一次有明确目标的 Copilot 对话。
- 指示代理对游戏卡片进行小幅更改。
- 使用 `/diff` 审阅更改。
- 运行应用，并在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，从筛选议题开始，使用 Plan 和 Autopilot 模式构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
