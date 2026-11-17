---
title: "练习 2 - 使用 Copilot 代理模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行测试等操作。开发者的常规工作包括查找所有相关文件、进行修改、运行测试、调试、找出遗漏文件并再次修改……类似步骤还有很多。

Copilot 代理模式正适用于这种场景。

Copilot 代理模式可以在集成开发环境（IDE）中更自主地工作。它的行为类似开发者：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在一起了解如何使用代理模式为网站添加新功能。

> [!NOTE]
> 名称相似，但代理模式和云端代理针对的是两种不同的体验。代理模式在 IDE 中执行任务，便于快速反馈和交互。云端代理则像团队中的同事一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 代理模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 代理模式如何跨用户界面（UI）和数据层实现新功能。
- 合并到代码库前如何审阅 Copilot 代理模式生成的更改和测试。

## 场景

随着游戏列表增加，你希望用户能够按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

先了解 Tailspin Toys 网站当前的功能，再进行任何更改。

该网站是一个以开发者为主题的棋盘游戏众筹平台，可用于列出游戏并显示详情。它是一个 Astro 应用，会在构建时将页面渲染为静态超文本标记语言（HTML）。页面通过 Drizzle 对象关系映射（ORM）在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端应用程序编程接口（API）或客户端用户界面（UI）框架。可复用的数据访问辅助函数位于 `src/lib/`，交互功能通过使用文档对象模型（DOM）API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习结束后，你应该位于 `main` 分支。
2. 在 Codespace 中打开新终端，按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 为筛选工作创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下内容的提示：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321`，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后在终端中选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace（现已托管网站服务器）的专用隧道！

### 浏览网站

网站运行后，可以浏览现有功能。主要功能包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，其中包含标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新对话。模式和模型选择因练习而异；每个相关步骤都会说明。

网站的初始实现已经可用，但还可以添加新功能。先查看待办事项。使用模板创建项目时，系统会自动创建一组议题；请让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并非推荐某一种模型。编写本工作坊时使用的是 Claude Sonnet 4.5，因此示例中也使用该模型，以便让代码建议相对一致。不过，大型语言模型（LLM）的结果具有概率性，你可能会看到不同的建议。这完全正常，也在预期之中。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问待办议题：

   ```plaintext
   请显示我 GitHub 存储库中的待办事项，并根据对用户最有用的程度帮助我确定优先级。
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 审阅生成的议题列表。

请注意，Copilot 还根据它认为对用户最有帮助的程度，为这些事项排定了优先级。

## 审阅说明文件

让代理生成代码前，先审阅将用于向 Copilot 提供上下文的说明文件。这里将使用[用户界面（UI）](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)说明文件，其中包含向网站添加功能的指导。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指南，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件既可以引用其他说明文件，也可以引用项目中的文件。路径相对于说明文件所在位置，因此可以复用内容、拆分较大的说明集，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要对应用进行以下三类更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页面，加入筛选 UI。

此外，合并前还必须运行并通过测试。Copilot 代理模式可以执行这些任务，现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 说明文件。

   ![选择 UI 说明文件的示例屏幕截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图代理下拉菜单中仍选择 **Agent**。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办议题实现功能：

   ```plaintext
   请根据待办事项中的相关 GitHub 议题，为网站添加按发行商和类别筛选的功能。上一练习已经添加发行商辅助函数，请保留并在需要时完善相关工作，同时完成剩余的数据层、UI 和测试工作。完成前确保所有测试通过。服务器已经运行，无需再次启动。
   ```

6. 观察 Copilot 探索项目并查找相关文件的过程。它应该会找到数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的屏幕截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。在此过程中，代码编辑器中可能会出现 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示相应的 **Keep** 或 **Undo** 按钮，因此无需在工作进行时立即选择。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板提示确认运行测试的屏幕截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，也可能会出现测试失败，这些情况都正常！Copilot 会在代码生成和测试之间反复迭代，直至任务完成且未发现错误。

   ![Copilot 代理模式完整 Chat 会话的屏幕截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 务必审阅 Copilot 或其他人工智能（AI）工具生成的代码。

10. 返回运行网站的浏览器，体验新功能。
11. 确认功能正常并审阅代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 代理模式如何跨 UI 和数据层实现新功能。
- Copilot 代理模式如何探索项目、识别相关文件并协调更改。
- 合并到代码库前如何审阅 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 - 实现分页

随着游戏列表扩大，需要添加分页。运用本练习所学知识，请 Copilot 更新网站以实现分页。编写提示时可考虑：

- 遵循现有最佳实践，包括使用现有说明文件。
- 考虑分页方式：允许用户选择每页大小，还是固定每页大小。
- 编写提示时，提供 Copilot 所需的指导，确保实现符合你的要求。
- 你可能需要与 GitHub Copilot 反复协作、提出更改并补充上下文。这是正常的协作流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
