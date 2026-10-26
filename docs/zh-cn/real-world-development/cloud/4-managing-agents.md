---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几节练习中，你请 Copilot cloud agent 处理了三个改善用户体验和添加功能的任务。Cloud agent 虽然可以异步自主工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给 cloud agent 的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，你可以查看所有带有未关闭 PR 的智能体任务、了解它们执行的操作，并引导正在运行的会话。

本课将：

- 使用智能体页面监控 cloud agent 任务。
- 引导正在运行的会话，请求添加更多功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也应该添加浅色模式。既然已经在更新网站样式并添加切换功能，不妨一并实现。你将引导智能体，同时实现浅色模式和高对比度模式。

## 查看 Copilot cloud agent 任务

查看当前分配给 Copilot cloud agent 的任务状态。

1. 打开 [Copilot agents 页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应该能看到分配给 Copilot 的任务，包括：
    - 更新代码库文档。
    - 为游戏创建修改 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 执行的操作，例如：
    - 从存储库检出代码。
    - 创建 Copilot 的工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的各项步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 当前的工作状态和处理方式而异。

4. 注意右侧显示的 PR 面板。你可以在此查看 PR 和已更改文件，以便进一步监控。

## 引导 cloud agent

现在已经看到正在运行的任务，可以在智能体实现高对比度模式的同时，请它添加浅色模式切换功能。

1. 选择有关添加高对比度模式的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它完成设置并开始工作。看到类似以下示例的消息时，说明已开始处理。
3. 在 **Steer active session while Copilot is working** 对话框中，添加以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Agents 页面中突出显示“Steer active session while Copilot is working”对话框的云智能体任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 注意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 等待 Copilot 完成

与之前一样，Copilot 会开始处理更新后的任务！它会在完成收到消息时正在执行的步骤后，将新请求纳入工作流程。

这同样需要几分钟。等待期间，可以回顾目前学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot agents 页面，它是监控和引导 GitHub Copilot cloud agent 任务的中心。你可以通过此控制中心跟踪所有正在运行和已完成的任务、查看执行过程，并重定向进行中的任务以调整范围或提供更多指导。

你了解了以下概念：

- 使用 mission control 和智能体页面监控 cloud agent 任务。
- 重定向正在运行的会话以请求更多功能。

Copilot 正在完成无障碍功能，现在可以开始[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot agents 页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
