---
title: "第 8 课 - 创建并合并功能 PR"
description: "一并审查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一分支上。现在一起审查这些内容并创建拉取请求。之前你自行合并了星级评分 PR；这次将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分为几个 PR。为简化本工作坊，你已将完整的筛选和质量工作流程保留在同一个会话和分支中，并统一放入此 PR。

本课将：

- 了解 Agent Merge 及其如何自动执行合并流程。
- 检查完整的功能 PR 和验证证据。
- 仅在完成审查后授权 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的整个工作流程中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动完成剩余的 PR 工作，同时让开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 可在 GitHub Copilot app 中自动处理合并拉取请求所需的后续工作。启用后，app 会话会读取 PR 并处理阻塞项，例如修复失败的持续集成（CI）检查、回复审查评论，以及在需要时进行 rebase。GitHub 允许后，它会立即合并 PR。该功能会在后台运行，应用重启后仍会继续，并在 PR 合并后自动关闭。

到目前为止，你一直自行选择 **Merge pull request**。Agent Merge 可以接手这项工作，但修改代码和合并仍需要你明确授权。授予合并权限前，请检查允许执行的操作和待处理工作。

## 使用 Agent Merge 管理 PR

所有代码都已创建并审查完成，现在让 Agent Merge 管理 PR 流程。

1. 使用智能体选择器选择 **Default agent**。
2. 选择 **Create PR** 旁边的下拉菜单。
3. 选择 **Agent Merge**。按钮会变为 **Agent Merge**。
4. 选择 **Agent Merge** 按钮，启动流程。仅在下拉菜单中选择该选项不会启动流程。

Agent Merge 流程将开始执行：

- 创建包含标题和说明的拉取请求。
- 如果会话从议题开始，则在说明正文中引用相关议题。
- 对目标分支进行 rebase，或处理潜在的合并冲突。
- 监控 CI 流程，确保所有检查通过。
- 监控 PR 中其他开发者或 Copilot 代码审查的反馈，并进行更新以解决评论。
- 可选择在所有检查成功后自动合并 PR。

现在让 Agent Merge 在所有检查通过后合并 PR。

5. 在授予合并权限前，审查 PR 差异和验证证据。
6. 选择 **Agent Merge** 旁边的下拉菜单。
7. 确认 **Merge pull request** 旁有勾选标记。
8. 明确要求智能体合并：

    ```plaintext
    Merge the PR once all required checks and approvals pass. Do not bypass repository protections.
    ```

9. 跟踪执行进度并解决报告的阻塞项。继续之前，确认 PR 状态为 **Merged**；启用合并权限或智能体仍在运行都不代表 PR 已成功合并。

> [!IMPORTANT]
> Agent Merge 不会绕过存储库保护或权限限制。继续前请先解决这些阻塞项。

## 总结和后续步骤

你已自动执行开发流程的多个环节，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解 Agent Merge 及其如何自动执行合并流程。
- 检查完整的功能 PR 和验证证据。
- 仅在审查后授权 Agent Merge，并确认 PR 已合并。

下一步将[使用现有画布并创建分类画布][next-lesson]，探索与智能体一起检查、规划和可视化工作的丰富方式。

## 资源

- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]
- [关于 GitHub Copilot app][about-copilot-app]

[next-lesson]: ../9-canvases/
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
