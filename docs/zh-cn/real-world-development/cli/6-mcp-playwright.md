---
title: "第 6 课 - 使用 Playwright 模型上下文协议（MCP）验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright 模型上下文协议（MCP），并使用它在浏览器中体验筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

前面已经提到，编写代码不只是写代码。我们还要处理数据、外部服务，并允许 Copilot 使用更多自动化功能。此时就需要 MCP 服务器。MCP 服务器可以扩展 Copilot CLI 内置的能力，为它提供更多工具和服务。

本课将完成以下目标：

- 了解模型上下文协议（Model Context Protocol，MCP）以及 Copilot CLI 如何使用它。
- 如果尚未配置 Playwright MCP 服务器，则添加该服务器。
- 请求代理操作浏览器并体验筛选功能。

## 场景

单元测试和端到端测试很重要，但要验证 UI 更新，还需要实际与 UI 交互。你希望 Copilot 能像用户一样使用正在开发的网站，进一步自动化更改流程，并增强对功能符合预期的信心。

## 什么是模型上下文协议（MCP）？

[模型上下文协议（MCP）][mcp-blog-post]为人工智能（AI）代理提供了与外部工具和服务通信的方式。借助 MCP，AI 代理可以实时访问外部工具和服务，从而获取最新信息并代表你执行操作。

代理通过 MCP 服务器访问这些工具和资源。MCP 服务器充当代理与外部工具、服务之间的桥梁。每个 MCP 服务器都代表 AI 代理可访问的一组工具和资源。

一些常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，帮助大家发现并为生态系统做出贡献。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先检查源代码、验证发布者，并评估安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 可以像用户一样操作网站。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux），保存新 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 体验功能

之前你已经手动确认功能行为符合预期。现在使用 Copilot 中的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示，要求 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   启动应用，并使用 Playwright MCP 根据议题和计划检查筛选功能。告诉我哪些行为正常、哪些不正常，不要进行任何更改。完成后停止你启动的服务器。
   ```

> [!NOTE]
> 通常不必明确要求 Copilot 使用 MCP 服务器，因为它往往会自行判断。但既然你知道应该使用哪个工具，明确指出也有帮助！这样结果会更一致，还能节省一些令牌。

2. 观察 Copilot 在浏览器中执行的步骤，确认功能是否正常。
3. 阅读报告，确保所有行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向你提供报告。

## 总结和后续步骤

恭喜！你已使用 Playwright MCP 服务器，通过 Copilot CLI 在真实浏览器中体验功能。回顾一下，你：

- 了解了模型上下文协议（MCP）以及 Copilot CLI 如何使用它。
- 在尚未配置 Playwright MCP 服务器时添加了它。
- 请求代理操作浏览器并体验筛选功能。

接下来，创建一个 QA 自定义代理，将技能和浏览器工具整合到专门的角色中。

## 资源

- [MCP 到底是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
