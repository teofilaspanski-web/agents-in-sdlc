---
title: "练习 3 - 自定义代理"
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

你将通过自定义代理学习以下内容：

- 自定义代理的定义方式。
- 如何向自定义代理分配任务。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视觉能力或偏好影响。最近的用户反馈指出，当前深色主题中文本与背景的对比度不足，导致部分用户难以阅读。为解决这一无障碍问题，设计团队要求实现可由用户开启或关闭的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。你将使用自定义代理生成所需功能。

## 审阅无障碍自定义代理

项目中已为无障碍工作创建了自定义代理。先查看其内容，了解它将如何引导 Copilot。

返回 codespace，查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 继续浏览并审阅以下部分：
   - 为无障碍网站生成代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 创建并分配议题

Mission control 是管理环境中所有代理的中心位置。你可以向 Copilot cloud agent 分配任务、监控任务进度，还可以重定向正在进行的会话并提供更多指导。现在先分配创建高对比度模式的任务。

1. 打开你的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建新议题。
5. 将 **Title** 设为 `Add high contrast mode to website`。
6. 将 **Description** 设为：

    ```plaintext
    我们需要为网站添加高对比度模式。应提供高对比度切换开关，供用户设置，并使用浏览器本地存储保存设置。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

    ![突出显示自定义代理和无障碍代理的云端代理分配屏幕截图](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务！

## 总结和后续步骤

本课介绍了 GitHub Copilot 中的[自定义代理][custom-agents]，即针对特定任务和领域定制的 AI 助手。通过自定义代理，团队可以将专业知识和标准编写为可复用代理，以便更有效地引导 Copilot 完成特定类型的工作。

你了解了：

- 自定义代理的定义方式。
- 如何向自定义代理分配任务。

Copilot 正在后台实现高对比度模式。接下来，前往 mission control [监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot cloud agent][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
