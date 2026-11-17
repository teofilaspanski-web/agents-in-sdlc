---
title: "第 4 课 - 使用自定义说明引导 Copilot"
description: "了解存储库说明，添加文档标准，并将该标准应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式人工智能（AI）时，上下文至关重要。如果任务需要以特定方式完成，应确保 Copilot 能够获取相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也描述代码应如何组织。现在筛选功能已经完成，你将了解 Copilot 使用的说明、添加一项文档标准，并将其应用于代码。

本课将完成以下目标：

- 了解存储库说明和按路径限定范围的说明文件如何提供给代理。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明其用途、参数和返回值；如果有可注入的 `db` 参数，也应予以说明。
- 可复用的 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

借助说明文件，可以确保 Copilot 掌握相关信息，并按这些实践完成任务。

## 说明文件

自定义说明可以向 Copilot 提供上下文和偏好，帮助它了解你的编码风格和要求。你可以通过指定编码约定、库以及注释风格来引导 Copilot，获得更相关的建议和代码片段。可以创建适用于整个存储库的说明，也可以为特定文件类型创建说明，以提供任务级上下文。

说明文件有两种：

- `.github/copilot-instructions.md`：单个说明文件，会随存储库中的**每个**请求发送给 Copilot。文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指南。

> [!NOTE]
> 不同环境支持的其他说明格式可能不同。使用特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看项目中的自定义说明文件

起始项目中已包含一些说明文件。先了解现有文件，再进行更改，以便观察更改的影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件，了解项目简介和编码指南。这些说明适用于 Copilot 在此存储库中的每次交互。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。文件涵盖 Astro、Drizzle 数据层、测试等内容。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它使用 glob 决定说明适用于哪些文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分为可复用的小文件，并提供可供参考的示例。

## 更新说明文件以符合团队指南

现有文件已经是不错的起点，但仍缺少一项要求。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的说明

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新说明。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的存储库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请求 Copilot 按更新后的指南修改代码：

   ```plaintext
   我们刚刚更新了说明和代码指南。请根据这些指南更新你生成的代码。
   ```

4. 输入 `/diff` 并阅读已更改的 TypeScript 文件。查看新生成的 TSDoc 注释，并确认其准确说明了代码。

## 总结和后续步骤

你了解了 Copilot 如何从说明文件获取上下文，并将新的标准应用于功能。具体来说，你：

- 了解存储库说明和按路径限定范围的说明文件如何提供给代理。
- 更新说明文件，确保遵循编码标准。
- 了解说明文件对代码的影响。

接下来，自定义并运行可复用的 `quality-checks` 技能，确保 lint 和测试能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
