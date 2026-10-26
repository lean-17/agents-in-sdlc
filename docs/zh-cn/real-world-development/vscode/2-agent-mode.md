---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发者通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏文件并再次修改……工作流程会不断延伸。

Copilot 智能体模式正是为此而设计的。

Copilot 智能体模式可以在 IDE 中更自主地工作。它的工作方式类似开发者：先探索现有项目结构，再进行必要更新、运行测试等任务，并自动修复发现的问题。接下来了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和 cloud agent 名称相似，但适用于两种不同的体验。智能体模式在 IDE 中执行任务，可快速反馈和交互；cloud agent 则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并代码前如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

游戏列表不断扩大，因此你希望用户能够按类别和发行商筛选。上一练习已经添加 publishers helper，现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

开始更改前，先浏览 Tailspin Toys 网站，了解当前功能。

该网站是一个带有开发者主题的棋盘游戏众筹平台，用户可以浏览游戏列表并查看详情。网站是单个 Astro 应用，页面在构建时会渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有独立后端 API 或客户端 UI 框架。可复用的数据访问 helper 位于 `src/lib/`，交互功能通过使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 命令，可启动唯一的 Astro 开发服务器，方便运行网站。按照以下步骤在 GitHub Codespace 中启动：

1. 返回 Codespace。上一个练习结束后，当前应位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 为筛选功能创建并切换到新分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，终端会显示类似以下内容的地址：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 Codespace 中选择终端里的 localhost 链接时，系统会自动将你重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 Codespace 的私有通道，现在你的 Codespace 正在托管 Web 服务器。

### 浏览网站

网站运行后，可以探索以下主要功能：

- **主页**：显示棋盘游戏列表及其标题、图像和描述。
- **游戏详情页**：选择游戏后会打开详情页，显示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索积压工作

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

网站的初始实现可以正常运行，但我们希望添加新功能。先查看积压工作。使用模板创建项目时，系统已自动创建一组议题；请 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体会自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本工作坊作者并未偏好任何特定模型。编写本工作坊时使用了 Claude Sonnet 4.5，因此将它列入说明，希望你获得相对一致的代码建议。但由于 LLM 具有概率性，收到的建议可能与本工作坊描述不同，这是正常且预期的情况。

> [!NOTE]
> 由于 LLM 具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 使用以下提示，请 Copilot 查看议题积压工作：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

请注意，Copilot 还根据对用户的实用程度对议题进行了排序。

## 查看说明文件

让智能体生成代码前，先查看用于向 Copilot 提供上下文的说明文件。本练习将使用 [用户界面（UI）说明][ui-instructions]，其中包含如何为网站添加功能的上下文。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看总体指导，包括：
   - 架构概览。
   - 组件设计、可测试性和无障碍原则。
   - 指向不同文件类型专用说明的链接，包括：
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 说明文件可以引用其他说明文件和项目文件。路径相对于说明文件所在位置，因此可以复用内容、拆分复杂说明，并提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三项单独更新：

- 在数据层（`src/lib/`）添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，加入筛选 UI。

此外，合并前必须运行测试并确保全部通过。Copilot 智能体模式可以替你完成这些任务。现在开始添加功能。

1. 可以继续当前 Copilot 对话，也可以选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions**，然后选择 `ui` 说明文件。

   ![选择 UI 说明文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍然选择了 **Agent**。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍然选择了 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据积压工作中相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何先探索项目并找到相关文件。它应找到数据层 helper、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多项任务，例如探索项目、修改文件和运行测试。根据任务和代码库的复杂程度，可能需要几分钟。过程中，代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，所有更改都会显示 **Keep** 或 **Undo** 选项；工作进行时无需选择。

7. 按 Copilot 的提示选择 **Continue**，运行测试。

   ![Copilot Chat 窗格中要求用户确认是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至看到某些测试失败。这没有问题！Copilot 会在生成代码和运行测试之间反复迭代，直到完成任务并且不再发现错误。

   ![Copilot 智能体模式完整聊天会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 始终要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，体验新功能。
11. 确认功能正常并检查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习介绍了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。你了解了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并代码前如何审查 Copilot 智能体模式生成的更改和测试。

下一步将[使用 Playwright MCP 服务器测试功能][next-lesson]，并为此创建拉取请求。

### 附加探索练习 — 实现分页

游戏列表不断扩大后，需要支持分页。运用本练习学到的技能，请 Copilot 更新网站以实现分页。需要考虑：

- 遵循现有最佳实践，包括使用已有的说明文件。
- 考虑分页方式，例如允许用户选择每页大小，或使用固定值。
- 编写提示时提供 Copilot 所需的指导，确保实现符合预期。
- 可能需要与 GitHub Copilot 迭代，通过请求更改并提供上下文来调整实现。这是使用 Copilot 的正常流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
