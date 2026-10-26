---
title: "练习 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义说明][previous-lesson] |
|:--|

许多组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因为一直没有时间而搁置在积压工作中的功能请求。GitHub Copilot cloud agent 可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发者审查。这样可以把繁琐工作交给智能体，加快开发进程，让开发者专注于更重要的工作。

你将了解 Copilot cloud agent 的以下用法：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些待处理的技术债务。最初承包网站开发工作的团队没有妥善编写文档，应用中几乎没有文档。首先，他们希望为应用中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏，即同一类别中的其他游戏，方便访客继续浏览。他们暂时不需要完善的实现，只需要可用于 UX 验收测试的版本。该任务目前受阻，而且优先级低于其他议题。

这类任务很容易被降级处理，因此非常适合交给 Copilot cloud agent。它可以异步处理任务，让开发者先做其他工作，稍后再回来审查 Copilot 的成果。

## 了解 GitHub Copilot cloud agent

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) 可以像人类开发者一样在后台执行任务。你可以通过多种方式分配工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置环境并开始处理任务。任务执行期间和完成后，你都可以查看 Copilot 会话。解决方案准备好供你审查时，Copilot 会在拉取请求中提及你。

## 编写范围明确的说明

GitHub Copilot 并没有什么魔法，使用它并不能省去思考。即使看似简单的操作，深入分析后也可能很复杂，因此要注意为 Copilot cloud agent 明确界定任务范围，并遵循[最佳实践][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、学习、试验并持续调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义说明

前面的练习介绍了自定义说明及其如何指导 Copilot。如果已经完成自定义说明练习，就见过 `.github/copilot-instructions.md` 的实际效果；否则，现在可以快速查看。分配任务前，先只读检查存储库中已有的说明文件，以便稍后观察其影响。

在存储库的 GitHub Web UI 或运行中的 Codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它将说明限定到 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给 cloud agent 时，留意生成的 PR 中是否遵循了这些文件中的 TSDoc、TypeScript 约定和注释头要求。后续练习审查 PR 时还会再次介绍。

## 为 Copilot cloud agent 设置开发环境

无论由谁完成，编写代码通常都需要特定环境和设置脚本，以确保一切就绪。将任务分配给 Copilot 时也是如此，它会像软件工程师一样执行任务。

Cloud agent 使用 [GitHub Actions][github-actions] 作为工作环境。你可以通过 `.github/workflows/copilot-setup-steps.yml` 中配置的[专用设置工作流][setup-workflow]来自定义环境，并在它开始工作前安装所需工具和依赖项。本实验已预先配置该工作流，以保证流程顺利并提供学习机会。它会确保 Copilot 能使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及单个 Astro 应用已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的“Actions”选项卡测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设置为这些步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 contents: read 权限。如果设置步骤不克隆存储库，Copilot 会在这些步骤完成后自动克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 - Node.js（整个应用现在基于 Astro + Drizzle/libSQL）
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

      # 迁移并填充本地 SQLite 数据库，以便构建和测试使用其中的数据。
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流相似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器，它允许你从存储库的 **Actions** 选项卡手动运行工作流。这样可以先测试工作流是否成功，而不必等待 Copilot 运行。

## 添加文档

虽然人人都知道文档的重要性，但大多数项目的文档不是过时，就是缺失。这样的技术债务经常得不到处理，会降低工作效率、增加维护难度，也让新开发者更难加入团队。Copilot 擅长生成文档，因此这是适合分配给 Copilot cloud agent 的议题。它会在后台生成所需文档，你将在后续练习中检查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求（PR），标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应该会在 PR 中创建待办事项列表。

> [!NOTE]
> Copilot 创建待办列表可能需要几分钟。Copilot 正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的方案。

13. 查看列表以及 Copilot 准备执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的指示器。

16. 查看实时会话，观察 Copilot 如何探索代码、了解当前状态、暂停思考并选择合适方案，以及创建代码。

此过程可能需要几分钟。Copilot cloud agent 的主要目标之一是异步执行任务，让我们能专注于其他工作。接下来利用这一点：再分配一个任务给 Copilot cloud agent，同时编写应用功能代码。

## 在游戏详情页添加相关游戏

如前所述，GitHub Copilot cloud agent 的一大优势是可以分配不同工作，让你在处理一组任务时，它同时处理另一组任务。为设计团队添加相关游戏区块可能不需要很长时间，但也可以利用这段时间完成其他工作。现在将此任务分配给 Copilot cloud agent。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，你会在议题的第一条评论中看到一组 👀，表示 Copilot 正在处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二个请求！它的工作方式类似软件工程师，因此无需一直监控，等它完成后再检查即可。现在转而了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot cloud agent][copilot-agents]。可以将议题分配给 cloud agent，让它异步处理技术债务、创建新功能，或协助将代码迁移到其他框架。

你了解了以下概念：

- 自定义代码生成环境。
- 确保操作安全。
- 明确界定议题范围。
- 将议题分配给 Copilot。

Copilot cloud agent 正在后台处理任务，现在可以开始创建和使用自定义智能体。[Copilot cloud agent 也支持 MCP 服务器][cloud-agent-mcp]，并可以使用前面模块介绍的自定义说明。

## 资源

- [关于 Copilot cloud agent][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
