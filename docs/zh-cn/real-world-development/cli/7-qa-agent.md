---
title: "第 7 课 - 创建和使用 QA 智能体"
description: "创建 QA 自定义智能体，结合议题要求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已经使用 `quality-checks` 技能运行自动检查，也使用 Playwright MCP 在浏览器中体验了筛选功能。现在，将这些能力组合到一个定义清晰的 QA 流程中。

本课将：

- 了解自定义智能体如何使用说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题检查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求（PR）前，以一致的方式检查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并生成可复用的报告。

## 什么是自定义智能体？

自定义智能体是在 Markdown 配置文件中定义的 Copilot 专用版本。配置文件描述智能体的用途、说明和可用工具。本工作坊会在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

前面使用的自定义功能各有用途：存储库说明描述团队标准；`quality-checks` 技能封装可重复的检查；Playwright MCP 提供浏览器工具；QA 配置则指导 Copilot 如何使用这些能力来评估需求并报告发现。它们各司其职，不会彼此替代，也不需要另开对话。

## 创建 QA 配置

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置。该配置会定义 QA 执行的检查以及必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新智能体前，先检查其配置，确认 Copilot 捕获了预期的 QA 流程和边界。这样可避免智能体在你只想验证功能时进行不完整或范围过大的更改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必需字段；`name` 是可选字段，但填写后会显示清晰的智能体名称。
3. 阅读配置说明，确认 QA 会从需求出发、遵循存储库说明、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据，修改实现代码前会先征求同意，并且不会提交更改或创建 PR。
5. 如果配置遗漏了任何职责或边界，请默认智能体先修改，再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置后，恢复同一筛选对话，再选择 QA，使其能够使用已有的议题和规划决定。

1. 使用以下提示启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能尚未显示在可用智能体列表中，但仍可通过上述命令启用。

2. 使用以下提示，请 QA 智能体检查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体完成工作。
4. 完成后阅读它提供的报告。

## 总结和后续步骤

你已添加可复用的专用角色，并检查了它的工作。本课中，你：

- 了解自定义智能体如何使用说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置。
- 选择 QA 智能体，并根据筛选议题检查其发现。

现在，实现、技能更新、QA 配置、测试和验证报告都已准备好接受审查。下一课将[汇总这些更改并创建功能 PR][next-lesson]，然后使用 Agent Merge。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
