---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债问题，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot cloud agent 可以自主完成更新代码、添加功能等任务。代理完成工作后，会创建草稿拉取请求，供开发者审阅。这样可以将繁琐任务交给代理，加快开发流程，让开发者专注于更重要的工作。

本课将使用 Copilot cloud agent 探索以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要处理的技术债。最初受聘开发网站的承包商没有留下完善的文档，事实上，代码完全缺少文档。首先，团队希望为应用中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现体验。他们想在每款游戏的详情页显示相关游戏（同一类别中的其他游戏），让用户可以继续浏览。现在不需要完善的实现，只需要一个可用于用户体验验收测试的版本。这项任务目前尚未排入最高优先级，但还有其他优先级更高的议题。

这两项任务都可能很快被降级，可以交给 Copilot cloud agent 异步处理。开发者可以先处理其他工作，之后再回来审阅 Copilot 的成果并确认一切符合预期。

## 认识 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。就像与人类开发者协作一样，有多种方式启动任务，其中一种是[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配任务后，Copilot 会创建草稿拉取请求以跟踪进度、设置环境并开始工作。会话进行中或完成后，都可以查看 Copilot 会话。提出的解决方案准备好供你审阅时，Copilot 会在拉取请求中提及你。

## 明确界定任务范围的重要性

GitHub Copilot 并非魔法工具，不能替你思考。看似简单的操作也可能包含复杂环节，因此[应谨慎界定分配给 Copilot cloud agent 的任务范围][cloud-agent-best-practices]。把它视为人工智能（AI）配对编程助手：分阶段工作、学习、尝试并逐步调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 查看此存储库中的自定义说明

前面的练习介绍了自定义说明以及它们如何引导 Copilot。如果已经完成自定义说明练习，就会看到 `.github/copilot-instructions.md` 的作用；如果尚未学习，现在正好快速查看一下。将任务分配给 Copilot cloud agent 前，先只读查看存储库中已有的说明文件，以便稍后观察它们的影响。

在 GitHub 网页界面或已运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解说明如何限定到 `**/*.test.ts` 文件。

在下一节将 `Code lacks documentation` 议题分配给 cloud agent 时，留意生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 约定和文件注释头。这些内容来自上述说明文件，稍后审阅 PR 时还会再次提及。

## 设置 Copilot cloud agent 的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，以确保环境状态良好。将任务分配给 Copilot 时也是如此；它会像软件工程师（SWE）一样执行工作。

Cloud agent 使用 [GitHub Actions][github-actions] 环境执行任务。你可以通过创建[专用设置工作流][setup-workflow]来自定义环境，该工作流配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并会在 cloud agent 开始工作前运行。这样可以提供所需的开发工具和依赖项。实验已预先配置此工作流，以便顺利完成练习并开展实践。它会为单个 Astro 应用安装 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 允许你从存储库的 "Actions" 选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 为你的步骤设置所需的最低权限。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果要在设置步骤中克隆存储库（例如安装依赖项），需要 `contents: read` 权限。如果设置步骤不克隆存储库，步骤完成后 Copilot 会自动为你克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时有可用数据。
      - name: Set up the database
        run: npm run db:setup
```

该文件看起来与其他 GitHub 工作流类似，但有几个关键点：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器。借助它，可以从存储库的 **Actions** 选项卡手动运行工作流。在等待 Copilot 运行它之前，先测试工作流是否成功很有帮助。

## 添加文档

大家都知道文档很重要，但多数项目的文档不是过时就是缺失。这类技术债往往长期得不到处理，降低工作效率，也增加维护代码库或帮助新开发者上手的难度。Copilot 擅长生成文档，因此这正是适合分配给 Copilot cloud agent 的议题。它会在后台生成所需文档，之后的练习会审阅其成果。

1. 在新的浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，创建新议题。
5. 将 **Title** 设为 `Code lacks documentation`。
6. 将 **Description** 设为：

    ```plaintext
    我们组织要求在适当情况下为函数和方法添加 TSDoc 文档注释。遗憾的是，最近的更新没有遵循此标准。我们需要更新现有代码，在文档注释有助于说明行为的地方添加注释。
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 任务分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求（PR），其标题类似 `[WIP]: Code lacks documentation`。如果列表中暂时没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会显示在 PR 中。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表和 Copilot 即将执行的任务。
14. 向下滚动拉取请求时间线，应能看到 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 浏览实时会话，查看 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及生成代码。

这项工作可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能处理其他工作。现在利用这一特点，再分配一项任务给 Copilot cloud agent，然后着手为应用编写新功能。

## 在游戏详情页添加相关游戏部分

如前所述，GitHub Copilot cloud agent 的一大优势是可以拆分工作：你可以专注于一项任务，同时让它处理另一项任务。即使为设计团队添加相关游戏部分不一定需要很长时间，也可以利用这段时间处理其他工作。现在将任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**，使用空白模板。
5. 将 **Title** 设为 `Show related games on the game details page`。
6. 将 **Description** 设为：

    ```markdown
    我们希望在每款游戏的详情页显示相关游戏，帮助用户发现更多游戏。设计团队想探索用户体验并开展验收测试。要求如下：

   - 在 `src/lib/` 中添加数据访问辅助函数，返回与指定游戏属于同一类别的其他游戏，但不包括指定游戏本身
   - 在游戏详情页显示“Related games”部分并使用该辅助函数
   - 处理游戏没有相关游戏的情况
   - 为新辅助函数创建单元测试
   - 创建 PR 前确保所有测试通过
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，议题的第一条评论中会出现一组 👀，表示 Copilot 已开始工作！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在努力处理第二项请求！Copilot cloud agent 的工作方式类似 SWE，因此无需主动监控；任务完成后再进行审阅即可。现在将注意力转向创建和使用自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将议题分配给 cloud agent，让它异步处理。Copilot 可用于解决技术债、创建新功能，或协助将代码从一种框架迁移到另一种框架。

你了解了：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

Cloud agent 正在后台工作，现在可以开始创建和使用自定义代理。[Copilot cloud agent 也可以使用 MCP 服务器][cloud-agent-mcp]，并可以使用之前练习中介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
