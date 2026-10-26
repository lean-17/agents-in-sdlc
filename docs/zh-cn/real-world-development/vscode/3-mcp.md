---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

你刚才使用智能体模式构建了筛选功能。创建拉取请求前，应先确认它能在浏览器中正常运行。不必自行操作应用，可以连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

本练习将：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 的工具。
- 将 Playwright MCP 服务器添加到工作区。
- 请 Copilot 使用它在浏览器中手动测试筛选功能。
- 发布分支并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

智能体模式可以通过 MCP 访问编辑器以外的资源。Model Context Protocol (MCP) 是智能体与外部工具和服务通信的标准方式。

![智能体模式内部工作原理示意图，显示它如何与上下文、LLM 和工具交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 智能体提供与外部工具和服务通信的方式。通过 MCP，AI 智能体可以实时访问最新信息（使用资源），并代表你执行操作（使用工具）。

智能体通过 MCP 服务器访问工具和资源。MCP 服务器负责管理 AI 智能体与外部工具之间的通信，例如现有 API 或 NPM 包等本地工具。每个 MCP 服务器提供一组智能体可以访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 存储库的 API，使 AI 智能体能够创建存储库、更新现有存储库以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，使 AI 智能体能够浏览网页、填写表单和选择按钮。

还有许多 MCP 服务器提供其他工具和资源。GitHub 托管了 [MCP 注册表](https://github.com/mcp)，帮助发现并为生态系统贡献内容。

> [!CAUTION]
> 从安全角度来看，请像对待其他项目依赖项一样对待 MCP 服务器。使用前应仔细检查源代码、验证发布者并考虑安全影响。只使用可信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行敏感操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 文件中应该已经配置了 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

此 `github` 项来自项目模板，因此 Copilot 在上一练习中可以读取议题积压工作。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装。VS Code 通过 HTTP 连接，并在 Copilot 首次使用其工具时要求你使用 GitHub 身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控浏览器，正好可以用于测试功能。

1. 在 `.vscode/mcp.json` 中与 `github` 并列添加 `playwright` 项，使文件如下所示：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标志会让 Playwright 在没有可见窗口的情况下运行浏览器，这对没有桌面环境的 Codespace 是必需的。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已经安装所需浏览器。如果 Copilot 报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器：Copilot 首次需要某个服务器的工具时，VS Code 会启动该服务器，并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 项上方会显示 **Start** 操作。选择它以立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请检查配置并选择信任，以便启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一部分首次需要服务器工具时启动它，并请求你信任服务器。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]为 Copilot 提供可控的真实浏览器。你无需自行操作应用来检查成果，智能体可以打开页面、浏览、应用筛选并读取结果，然后总结观察内容。无需离开对话，就能快速确认功能行为是否符合预期。

Playwright MCP 服务器在底层使用页面的[无障碍树][playwright-mcp-server]，而不是截图。这意味着智能体会像辅助技术一样，根据带标签的结构化元素（按钮、链接、列表项）进行分析，因此简单的功能检查也能顺带进行基本的无障碍检查。

连接服务器并启动应用后，请 Copilot 测试刚实现的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器并逐项执行测试，然后报告结果。对照议题中的验收标准查看报告。如果发现问题，请继续提问，或让 Copilot 在创建 PR 前修复代码。

> [!NOTE]
> 此测试要求应用运行在 `http://localhost:4321`。如果开发服务器已停止，请在发送提示前重新启动。Copilot 首次使用 Playwright MCP 服务器时可能需要下载浏览器；如果报告缺少浏览器，请让它运行 `npx playwright install chromium` 并重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求（PR），供团队审查。首先发布 `filtering-vscode` 分支。

1. 打开 Codespace 中的 **Source Control** 面板，检查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面（CLI）创建拉取请求。由于你已经在使用 GitHub Copilot，不妨让它代为创建 PR！它可以找到相关议题，并在 PR 中关联该议题。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认智能体下拉菜单中选择了 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 使用以下提示请 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行所需操作以收集信息和执行任务。首次使用 GitHub 工具时，系统可能会提示你进行 GitHub 身份验证；按照提示授权。
5. 观察 Copilot 如何搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 提供的链接查看拉取请求，但**暂时不要合并**。

## 总结和后续步骤

恭喜！本练习中，你：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 的工具。
- 将 Playwright MCP 服务器添加到工作区。
- 使用它在浏览器中手动测试筛选功能，然后发布更改。
- 发布分支并为筛选功能创建拉取请求。

你使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。下一步将[创建自定义智能体][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
