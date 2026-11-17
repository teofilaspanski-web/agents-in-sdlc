---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot cloud agent →][next-lesson]

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解一些背景信息，就应确保它能够获取相应上下文。[说明文件][instruction-files]可以提供这些指导，让 Copilot 不仅了解要做**什么**，也了解应该**如何**完成。

本练习将完成以下目标：

- 了解项目上下文、编码指南和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件提供给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的存储库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会运行“更新前/更新后”的提示进行对比。Copilot cloud agent 会在 GitHub 议题上异步工作，因此难以实时并排展示影响。稍后审阅 cloud agent 创建的拉取请求时，可以看到说明文件的作用。

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
> Copilot 也可以帮助生成说明文件。不同界面的操作方式不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。本学习路径会在相关步骤说明当前界面的操作。
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

## 添加新的存储库标准

接下来只需编辑一处：添加一项项目级规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。之后，本学习路径中你分配给 Copilot 的议题都会采用这项标准。

编辑前先设置分支。（云端代理会从议题目标分支读取说明，但仍然应在独立分支中提交更改。）

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项编码标准，提高代码建议质量。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应位于第 27 行附近。注意该部分记录了项目编码标准，但尚未规定代码文档要求，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指南，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指南。存储库级文件适合放置项目级标准，例如刚刚添加的文档注释要求。

## 提交、推送并合并说明更新

Copilot cloud agent 会读取议题目标分支中的说明文件。下一练习中，将议题分配给 Copilot 时，它会从 `main` 分支创建分支，因此必须先将说明更改合并到 `main`，云端代理才能使用这些说明。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并合并该请求。这样，云端代理在下一练习中处理议题时就会读取这些说明。

> [!TIP]
> 如果本工作坊中希望直接操作 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你了解了 Copilot 如何从项目说明文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了新的存储库级标准。后续练习中，可以在 Copilot cloud agent 创建的拉取请求中看到该标准的效果。

接下来，将第一个议题分配给 Copilot cloud agent。

## 资源

- [用于自定义 GitHub Copilot 的说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
