---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能，自定义其报告格式，并用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码不只是编写代码。我们已经手动验证代码能否运行，并使用说明文件确保代码符合标准。那测试、lint 和其他持续集成 (CI) 工作该怎么处理？

**智能体技能**最适合执行这些任务！技能可以帮助 Copilot 正确运行此类操作。

本课将：

- 探索现有的 `quality-checks` 技能。
- 自定义技能的结果报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更容易阅读。

## 说明、脚本和资源

智能体技能将可复用的任务说明、可执行脚本和辅助资源打包在一起，由智能体按需加载。技能本质上是一个以技能名称命名的文件夹，其中包含 Markdown 文件 `SKILL.md`。该文件通过 frontmatter 中的名称和描述定义技能，说明其功能并指导何时调用。文件夹中还可以包含供技能使用的脚本和其他资源。

> [!NOTE]
> 技能不一定需要其他文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它会使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能文件夹 `~/.copilot/skills` 中。

## 探索技能

来看看 Tailspin Toys 团队为运行单元测试、lint 和类型检查创建的 `quality-checks` 技能。

1. 返回 Codespace，并在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述会帮助 Copilot 判断何时调用此技能。
3. 阅读说明，了解它如何指导 Copilot 执行测试和 lint 流程。
4. 注意技能中尚无 **Results output formatting** 部分。

## 修改前先运行技能

可以在 Copilot CLI 中直接调用技能，也可以使用自然语言请求。先请 Copilot 运行技能中的三项检查。

1. 返回筛选功能对话，并确认处于 Interactive 模式。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告。

## 自定义报告

我们希望报告能说明运行了哪些检查、是否成功，以及工具实际报告了什么。现在更新技能，让它按此格式生成报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

现在通过 Copilot CLI 体验更新后的技能。编辑技能后，无需重启对话即可重新加载。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 再次使用相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一次报告比较。

## 总结和后续步骤

你已自定义并运行现有智能体技能。本课中，你：

- 探索了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了结果报告格式。
- 重新加载并运行了技能。

这项更改将与筛选功能一起纳入功能 PR。下一课将通过 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例可供参考，不是额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流程：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
