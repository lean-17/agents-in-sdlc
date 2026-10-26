---
title: "第 1 课 - 安装 GitHub Copilot CLI"
description: "在 Codespace 中安装并验证 Copilot CLI、熟悉基本操作，并找到预置的筛选议题。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] 是一款强大的智能体式编码助手，可在终端中运行，让你通过命令行探索代码库、生成代码、运行命令并使用外部工具。它可以分担任务、根据请求修改内容，并帮助你专注工作。第一步自然是安装工具！好在可以使用熟悉的工具完成安装。

本课将：

- 使用 npm 安装 GitHub Copilot CLI。
- 使用 GitHub 帐户完成身份验证。
- 信任工作坊存储库并尝试快速对话。
- 通过内置 GitHub MCP 服务器查找筛选议题。

## 场景

团队开始使用 AI 智能体处理不断增长的积压工作。Copilot CLI 将这种能力带入终端，而终端是许多开发者日常工作的主要环境。本课将完成安装和身份验证，为后续工作坊做好准备。

## 安装 Copilot CLI

可以通过 [npm][install-cli]、WinGet 和 Homebrew 安装 Copilot CLI。GitHub Codespaces 已预装 Node.js，因此本练习使用 npm。

1. 返回 Codespace 并打开终端。
2. 确认已安装 Node.js，且版本符合要求：

   ```bash
   node --version
   ```

   应该会显示版本 24 或更高。

3. 全局安装 Copilot CLI：

   ```bash
   npm install -g @github/copilot
   ```

4. 验证安装：

   ```bash
   copilot --version
   ```

   应该会显示版本号。

## 使用 GitHub 完成身份验证

首次启动时，Copilot CLI 会提示你使用 GitHub 帐户完成身份验证。

1. 启动 Copilot CLI：

   ```bash
   copilot
   ```

2. 如果系统提示，请按照设备代码说明完成身份验证并授权 Copilot CLI。
3. Copilot CLI 会显示以下提示：

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 确认显示的路径是 Tailspin Toys 存储库，然后选择 **Yes, and remember this folder for future sessions**，表示信任此目录。

> [!NOTE]
> 在 Codespace 中，你可能已经通过 GitHub 会话完成身份验证。如果 Copilot CLI 启动时没有提示身份验证，就可以直接使用。

## 熟悉基本操作

在普通 shell 提示符中输入的命令会直接在 Codespace 中运行。Copilot CLI 启动后，自然语言会发送给智能体，斜杠命令则用于控制对话。

1. 输入 `/model`，使用方向键选择 **Auto**，按 <kbd>Enter</kbd>，然后再次按 <kbd>Enter</kbd> 确认。
2. 输入 `/help`，查看已安装版本提供的命令，然后按 <kbd>Esc</kbd> 关闭帮助界面。
3. 向 Copilot 提出一个简单问题，确认功能正常：

   ```plaintext
   What are the key files in this project?
   ```

4. 阅读回复，观察 Copilot 如何先探索存储库再回答。
5. 输入 `/mcp list`，确认内置的 GitHub MCP 服务器可用。
6. 使用 GitHub MCP 请 Copilot 查找筛选议题：

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. 打开 URL 并阅读议题。完成首次小改动后会用到它。

> [!TIP]
> 普通 Copilot CLI 会话使用终端当前检出的分支，不会自动创建工作树。每次更改前都会创建功能分支。

## 使用工作坊快捷选项

Copilot CLI 通常会在使用现有权限范围之外的工具前询问许可。本工作坊将在 Codespace 中使用 `--yolo` 重新启动 CLI，以移除这些确认提示，便于专注练习。

> [!CAUTION]
> `--yolo` 会启用完整的自动权限（`--allow-all-tools`、`--allow-all-paths` 和 `--allow-all-urls`）。只能在 Codespace 或 VM 这类隔离环境中使用，绝不要将它设置为日常开发的默认别名。详情请参阅[允许和拒绝工具使用][allow-all-warning]。

本工作坊还会使用 `--enable-all-github-mcp-tools` 启用后续课程操作议题和拉取请求所需的 GitHub MCP 读写工具。Codespace 限制了对本地计算机的访问，但经过身份验证的 GitHub 资源仍然是真实资源。发布或合并更改前，请先进行检查。

1. 输入 `/exit` 退出 Copilot CLI。
2. 在存储库根目录重新启动：

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. 再问一个有关项目的简单问题，确认对话正常，然后输入 `/exit` 退出。

Copilot 会自动保存对话。后续更改说明或添加智能体后，可以使用 `copilot --resume` 返回同一功能对话和分支。

## 总结和后续步骤

恭喜！本课中，你：

- 使用 npm 安装了 GitHub Copilot CLI。
- 使用 GitHub 帐户完成了身份验证。
- 信任了工作坊存储库并尝试了快速对话。
- 通过内置的 GitHub MCP 服务器找到了筛选议题。

下一步将[开始第一个范围明确的更改][next-lesson]，并使用 Copilot CLI 在游戏卡片上显示星级评分。

## 资源

- [安装 GitHub Copilot CLI][install-cli]
- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
