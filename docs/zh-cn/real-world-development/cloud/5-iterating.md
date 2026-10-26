---
title: "练习 5：迭代 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 合作完成了多个改善用户体验和添加功能的任务：为代码添加文档、实现设计团队可继续迭代的相关游戏功能，以及添加高对比度和浅色模式等无障碍功能。现在检查代码更改，必要时向 Copilot 提供反馈以改进结果。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。因此，先审查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能提出的更改，再允许团队其他成员审查。

## 安全性和 GitHub Copilot cloud agent

Copilot cloud agent 会异步且无人监督地执行任务，因此需要采取安全限制来确保操作安全，包括：

- Copilot 只能读取存储库，并且只能写入用于处理代码的分支。
- Cloud agent 在 GitHub Actions 中运行，并在独立的临时环境中工作。
- 任何 GitHub Actions 工作流都必须由人工批准后才能运行。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先查看 GitHub Copilot cloud agent 创建的第一个 PR：为代码添加文档。使用 GitHub.com 上的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为默认情况下 Copilot 对外部资源的访问受到限制，包括对外部 MCP 服务器的调用。你可以选择[自定义或禁用 Copilot cloud agent 防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。你可以休息一会儿，或回顾已学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并检查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看更新后的代码，其中包含新增的 TSDoc 文档注释和其他文档。具体更改可能不同。检查 TSDoc 文档注释、TypeScript 约定和注释头，这些要求来自练习 2 开始时查看的自定义说明文件。
6. 检查完成且结果符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队。正常情况下，单个 Astro 应用的项目检查会通过。完成可能需要几分钟。

## 请求 GitHub Copilot 进行更改

与 Copilot 协作处理 PR 并不是单向过程。你可以像在 PR 评论或代码行内评论中提及团队成员一样提及 Copilot。Copilot 会读取评论，并启动新会话来处理请求。由于结果具有非确定性，无法提供唯一正确的提示文本。可以要求 Copilot：

- 在每个代码文件开头添加简短的注释头，说明文件用途。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用结构。

1. 添加评论并提及 **@copilot**，请求它修改生成的文档。可以使用以上任意建议，也可以提出其他文档改进要求。
2. 选择 **View Session**，观察 Copilot 如何开始新会话并进行更新。
3. 选择 **Back to pull request** 返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。
5. 选择 **Files changed** 选项卡，检查更改。

可以继续迭代，直到结果符合预期。满意后，可以将 PR 从草稿转换为可审查状态，然后合并到主分支。

![将 PR 转为可审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

返回 Copilot 为“在游戏详情页显示相关游戏”议题创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查完成且结果符合预期后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查区域中排队。正常情况下，单个 Astro 应用的项目检查会通过。完成可能需要几分钟。
9. **可选：** 可以在 Codespace 中切换到此分支，手动测试相关游戏功能。打开 Codespace 终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能！与之前一样，可以通过向 Copilot cloud agent 请求更新来迭代。例如，可以调整显示的相关游戏数量，或要求添加注释头和 TSDoc 文档注释（请记住，分配此任务时尚未更新自定义说明）。在 **Conversation** 选项卡中添加新评论即可提出请求，Copilot 会启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义智能体实现的功能。此 PR 应包含练习 3 中请求的高对比度模式，以及练习 4 中通过 mission control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]** 标记。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现时，重点关注：
   - 用于切换显示模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式对应的 CSS 或样式更改。
   - 无障碍属性，例如 ARIA 标签和键盘导航。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 检查完成且结果符合预期后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果工作流等待批准，请选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队。正常情况下，单个 Astro 应用的项目检查会通过。完成可能需要几分钟。
10. **可选：** 可以在 Codespace 中切换到此分支，手动测试无障碍功能。打开 Codespace 终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换，确认功能符合预期且页面重新加载后设置仍然保留。

无障碍自定义智能体帮助 Copilot 按照无障碍最佳实践实现这些功能。如果发现无障碍问题或改进空间，可以像处理前面的 PR 一样，在评论中提及 **@copilot** 请求更新。

## 可选练习 — 继续委派

Cloud agent 很适合处理真实积压事项，让你可以转而完成其他工作。可以为自己的存储库创建更多议题并分配给 Copilot，培养这种工作习惯。例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问 helper 添加输入验证和错误处理。

## 总结

你已完成 Cloud agent 工作流程。在这些课程中，你：

- **检查了存储库附带的自定义说明文件**，以便稍后了解它们对 cloud agent 输出的影响。
- **将议题分配给 Copilot cloud agent**，并观察它设置环境、规划和异步执行任务。
- **创建并使用无障碍自定义智能体**，实现高对比度和浅色模式切换。
- **使用 Copilot agents 页面作为控制中心**，监控并引导正在运行的无障碍会话。
- **审查并迭代 cloud agent 的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 审查和后续步骤

你已完成 Cloud agent 工作流程。若想继续探索，其他工作流程可以补充本次实践：

- 🖥️ **[VS Code 工作流程](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 工作流程](../../cli/)** — 在终端中交付两个经过审查的更改，学习 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体衔接方式。

还可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给 cloud agent。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot cloud agent 设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot cloud agent 防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
