---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]可用于创建专门的人工智能（AI）助手，针对开发工作流程中的特定任务或领域进行定制。在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义代理，可以为 Copilot 提供专门的说明、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识编写成可复用的代理，例如确保符合[《Web 内容无障碍指南》（WCAG）][wcag]要求的无障碍代理、遵循安全编码实践的安全代理，或保持测试模式一致的测试代理。

自定义代理通过项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局路径 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，至少包含 `name` 和 `description`，后面是定义代理行为、专长和说明的 Markdown 提示。

### 自定义代理与代理技能

自定义代理和[代理技能][agent-skills-concept]有些相似。两者主要通过 Markdown 文件定义，并告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理**是执行任务的工作者，**技能**是工具。

自定义代理有自己的上下文窗口，能够协调技能（甚至其他代理）来完成任务。在本实验中，无障碍自定义代理会根据无障碍指南审阅并更新网站；执行过程中，它可以调用拉取请求工作流程技能或运行和管理测试的技能。

> [!NOTE]
> 编写自定义代理没有唯一的正确方式。和 AI 的其他用法一样，需要在自己的环境和场景中测试并不断迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。最近的用户反馈指出，当前深色主题中文本与背景的对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义代理生成所需功能。

本练习将完成以下目标：

- 审阅现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 审阅无障碍自定义代理

项目中已为无障碍工作创建了自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，打开终端，并从 `main` 分支创建新的无障碍工作分支（筛选 PR 将与本次工作分开）：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 继续浏览并审阅以下部分：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理后，该聊天会话将采用代理的说明和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的新对话。模式和模型选择因练习而异；每个相关步骤都会说明。

1. 如果代理下拉菜单中尚未选择 **Agent**，请先选择它。

   ![显示 Chat 视图中代理选择器的屏幕截图。](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（默认情况下显示当前代理 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示：

    ```plaintext
    为网站添加高对比度模式。应提供高对比度切换开关，供用户设置；使用浏览器本地存储，以便重新加载页面后仍保留设置。
    ```

5. Copilot Chat 会开始工作：探索代码库、提出更改并编辑项目文件。每项编辑都会在聊天中内联显示文件路径和差异，供你审阅。
6. 随着编辑进行，**Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 此过程可能需要几分钟。Copilot 会实际修改存储库，例如编辑 Astro 组件、CSS 和相关测试。下一练习将审阅并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。通过自定义代理，团队可以将专业知识和标准编写为可复用代理，以便更有效地引导 Copilot 完成特定类型的工作。

你了解了：

- 自定义代理的定义方式。
- 如何在 Copilot Chat 代理模式中使用自定义代理。

接下来，[监控并引导代理工作][next-lesson]，在更改生成过程中进行审阅，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [Awesome Copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
