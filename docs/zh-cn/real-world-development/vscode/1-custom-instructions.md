---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中一种重要方式是使用[说明文件][instruction-files]，指导 Copilot 如何生成代码。

本练习将完成以下目标：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 使用当前说明生成筛选功能的第一个数据切片（发行商辅助函数）。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和筛选功能切片提交到 `main`，以便 Copilot 在下一练习中使用更新后的指南。

> [!CAUTION]
> 生成的代码可能不符合你设定的某些标准。Copilot 的结果具有概率性。本练习旨在观察更新说明后行为的整体变化趋势，而不是要求输出逐字一致。

## 说明文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- 用户界面（UI）应采用深色模式并具有现代感。
- 代码应使用 TSDoc 文档注释。
- 每个文件开头都应有一段注释，说明文件的用途。

通过说明文件，可以确保 Copilot 掌握相关信息，并按这些实践完成任务。

### 自定义说明

自定义说明可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以通过指定编码约定、库以及注释风格来引导 Copilot，获得更相关的建议和代码片段。可以创建适用于整个存储库的说明，也可以为特定文件类型创建说明，以提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：单个说明文件，会随存储库中的**每个**请求发送给 Copilot。文件应包含项目级信息，即大多数 Copilot 聊天或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指南。
- `.github/instructions/*.instructions.md`：可为特定任务或文件类型创建此类文件。可为 TypeScript 或 Astro 等语言以及创建 UI 组件或单元测试等任务提供指南。

> [!NOTE]
> 在集成开发环境（IDE）中，说明文件仅用于 Copilot Chat 代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级说明和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论如何创建说明文件超出了本工作坊的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 将 `copilot-instructions.md` 限定为项目级指南，例如项目用途、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持说明清楚，并提供代码应如何编写或不应如何编写的示例。

说明文件没有唯一的正确写法，AI 的使用方式也不例外。通过实践，你会找到适合自己项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的说明文件。查看本项目中的文件时，你可能会发现有些文件针对多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本练习会在相关步骤说明当前界面的操作。
>
> 想查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中收录了说明文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义说明文件

花些时间查看此存储库附带的说明文件，包括一个核心 `copilot-instructions.md` 文件和一组用于不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub 网页界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。查看 **Code standards** 中的嵌套 **GitHub Actions Workflows** 指南。这些说明适用于你与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹并浏览其中内容。文件涵盖 Astro、Drizzle 数据层、测试等内容。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于确定适用文件。此处，任何 TypeScript 测试文件（例如匹配 `**/*.test.ts` 的文件）都会匹配。
5. 查看此项目的单元测试专用说明。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件底部。查看其中指向其他说明文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的说明集拆分成更小且可复用的文件，并为 Copilot 提供可参考的示例。（这些路径相对于说明文件所在位置，而不是存储库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但目前没有要求在代码中编写文档。接下来会添加 TSDoc 文档注释和文件注释头的规则。

## 为更改创建分支

按照最佳实践，为更改创建新分支。

1. 返回上一练习使用的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新对话。模式和模型选择因练习而异；每个相关步骤都会说明。

要查看自定义说明的影响，先在现有说明不变的情况下发送提示。稍后更新说明，再次发送相同提示并比较结果。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取你需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，以便 Copilot 应用文件更改。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的列表，其中每个发行商都包含名称和 ID。应用文件更改。
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件及其测试。
6. 注意，建议的辅助函数是类型化函数：它将 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意，建议代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 的结果具有概率性，因此它有时可能会在未明确要求的情况下添加文档注释。这也没问题；本练习重点是观察更新说明后行为的一致性改进。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项编码标准，提高代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意该部分记录了项目编码标准，但尚未规定代码文档要求，因此刚才生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指南，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指南。存储库级文件适合放置项目级标准，例如刚刚添加的文档注释要求。

## 再次运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清除上下文。
2. 再次选择 `src/lib/publishers.ts`，让 Copilot 聚焦正确文件。
3. 发送与之前相同的提示：

   ```plaintext
   创建或更新 src/lib/publishers.ts，添加一个数据访问辅助函数，返回所有发行商的列表，其中每个发行商都包含名称和 ID。应用文件更改。
   ```

4. 注意现在建议文件开头会出现类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意现在建议的函数中包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚刚引导 Copilot 遵循新的项目标准，并将其应用到真实代码中；下一练习会在此基础上继续构建。

## 提交说明并推送分支

说明文件和存储库中的其他资源一样，都应使用相同的源代码管理流程。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux），打开新终端窗口。
2. 在终端中运行以下命令，确认说明更新和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交说明更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他支持性更新（例如测试），也要添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求并将其关联到待办事项中的文档标准议题。也可以手动操作，不过 Copilot 可以代为查找议题、创建用于关闭该议题的 PR，并合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你在 GitHub 上操作。项目模板已为工作区连接 GitHub 工具，因此无需其他设置。Copilot 首次使用工具时，VS Code 可能会提示你登录 GitHub。按照提示授权即可。稍后的练习会介绍这种连接所用的模型上下文协议（MCP）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认代理选择器中已选择 **Agent**。
3. 请求 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中包括说明更新和新的发行商辅助函数基础：

   ```
   查找与更新说明文件相关的议题。从当前分支创建新 PR，说明此 PR 会关闭该议题，并注明我们还为后续筛选工作添加了发行商辅助函数基础。
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 提示允许它代表你执行 GitHub 操作时，检查命令并酌情选择 **Allow**。
6. PR 创建后，使用以下提示请求 Copilot 合并 PR 并将分支切回 `main`：

   ```
   将 PR 合并到 main。然后在本地切回 main 并拉取最新代码，使本地保持同步。
   ```

7. Copilot 提示允许它代表你执行 GitHub 操作和运行 shell 命令时，检查命令并酌情选择 **Allow**。

现在，你已在 GitHub Copilot 的帮助下创建并合并了拉取请求！

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件中获取上下文，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示，观察 Copilot 根据**现有**说明生成的代码。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。
- 再次运行相同提示，观察生成的代码如何采用新标准。
- 将说明更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，使用[代理模式][next-lesson]让 Copilot 根据这些说明跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 的个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
