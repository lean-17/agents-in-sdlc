---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起检查筛选功能、说明、技能更新、QA 配置和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、说明更新、技能更新、质量保证（QA）配置和测试都保存在同一分支上。现在可以一起检查并创建拉取请求了。之前你亲自合并了星级评分 PR；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、说明更新、技能更新和 QA 智能体拆分为几个 PR。为简化本工作坊，你将完整的筛选功能和质量工作流程保留在同一对话和分支中，并放入同一个 PR。

本课将：

- 了解 Agent Merge 及其如何自动执行合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 完成审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能的整个工作流程中，你使用 Copilot 规划、实现并验证了功能。现在 Tailspin Toys 希望自动完成剩余的 PR 工作，同时让开发者掌控合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的后续工作。启用后，Copilot 会处理阻止 PR 合并的问题，例如修复失败的持续集成（CI）检查、回复审查意见，以及在需要时进行 rebase；之后，在存储库允许的情况下启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以接手这项工作。启用前，请先检查更改并确认已准备好合并。

## 使用 Agent Merge 管理 PR

代码已全部生成。现在一起检查代码、创建 PR，并让 Agent Merge 处理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、存储库设置或权限限制。如果合并受阻，请阅读报告中的原因，并在存储库允许时手动完成审查后的合并。

## 总结和后续步骤

你已自动执行开发流程的多个部分，包括生成代码、测试和验证代码，以及处理拉取请求。本课中，你：

- 了解 Agent Merge 及其如何自动执行合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 仅在完成审查后启用 Agent Merge，并确认 PR 已合并。

下一课将[了解更多实用的 Copilot CLI 斜杠命令][next-lesson]，用于管理上下文、模型、共享和可选的云委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
