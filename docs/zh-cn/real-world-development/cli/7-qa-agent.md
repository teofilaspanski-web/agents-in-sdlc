---
title: "第 7 课 - 创建并使用质量保证（QA）代理"
description: "创建一个质量保证（QA）自定义代理，整合议题要求、quality-checks 技能和 Playwright 模型上下文协议（MCP）。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，也使用 Playwright 模型上下文协议（MCP）在浏览器中观察筛选功能。现在将这些能力整合到一个具有明确的质量保证（QA）流程的自定义代理中。

本课将完成以下目标：

- 了解自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 代理，并根据筛选议题审阅其发现。

## 场景

在创建拉取请求（PR）前，Tailspin Toys 希望统一检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义代理可以协调这一 QA 流程，并提供可复用的报告。

## 什么是自定义代理？

自定义代理是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。本工作坊将通过 `.github/agents/qa.agent.md` 定义 QA 角色，并在 Copilot CLI 中选择该代理。

你使用的不同自定义机制各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复执行的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何使用这些能力评估需求并报告发现。QA 配置不会取代这些机制，也不需要另开对话。

## 创建 QA 配置

在打开功能 PR 前，请 Copilot 创建可复用的 QA 配置。配置中应明确 QA 执行的检查和必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请求默认代理创建新的自定义代理：

   ```plaintext
   在 .github/agents/qa.agent.md 中创建名为 QA 的自定义代理。它应根据议题和已商定的要求检查功能，遵循存储库说明，运行 quality-checks 技能，使用 Playwright MCP 验证行为，并在测试覆盖不足时添加测试。

   对每项要求，它都应提供证据并报告通过、失败或受阻。更改实现代码前必须征求同意，并且不得提交更改或创建拉取请求。

   现在只创建配置文件，供我审阅。
   ```

## 检查配置文件

使用新代理前，先审阅其配置，确认 Copilot 捕获了预期的 QA 流程和边界。这样可以避免配置不完整或范围过宽，导致只想验证功能时却修改了代码。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但添加后代理会有清晰的显示名称。
3. 阅读配置说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会征求同意，并且不会提交更改或创建拉取请求。
5. 如果生成的配置缺少上述职责或边界，请在继续前要求默认代理修改。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置文件后，恢复同一个筛选对话并选择 QA，这样它就能使用已有的议题和规划决定。

1. 使用以下命令启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚刚创建了代理，它可能不会出现在可用代理列表中。该代理仍然存在，上述命令会启用它。

2. 使用以下提示，请 QA 代理检查功能：

   ```plaintext
   根据议题和计划中的决定审查筛选功能。现在可以创建 PR 了吗？
   ```

3. QA 代理会开始工作。
4. 完成后，阅读它提供的报告。

## 总结和后续步骤

你已将可复用的专门角色加入工作流程，并审阅了它的工作。本课中，你：

- 了解自定义代理如何配合说明、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 代理，并根据筛选议题审阅了它的发现。

现在，功能实现、技能更新、QA 配置、测试和验证报告都已准备好供审阅。接下来，将这些成果整合到功能 PR 中，并使用 Agent Merge。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
