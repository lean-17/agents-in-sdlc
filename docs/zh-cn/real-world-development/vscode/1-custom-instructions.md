---
title: "练习 1 - 自定义说明（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些上下文。可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种是[说明文件][instruction-files]，用于指导代码生成。

本练习将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 在当前说明的基础上，生成筛选功能的第一块数据切片（publishers helper）。
- 在 `.github/copilot-instructions.md` 中添加新的全库标准。
- 再次运行相同提示，观察生成代码如何采用新标准。
- 将说明更新和筛选功能数据切片提交到 `main`，供下一练习中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不完全遵循某些标准。Copilot 具有概率性而非确定性。本练习的重点是观察更新说明后的行为趋势，而不是要求输出逐字一致。

## 说明文件

### 场景

与优秀的开发团队一样，Tailspin Toys 也有一套开发准则和要求，包括：

- 数据层始终需要单元测试。
- UI 应采用深色模式，并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明文件用途。

借助说明文件，可以确保 Copilot 掌握正确的信息，并按照这些实践完成任务。

### 自定义说明

自定义说明可以为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。说明文件可以指定编码约定、库以及希望包含的注释类型。说明既可以针对整个存储库，也可以针对特定文件类型，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：针对存储库的单个说明文件，每次请求都会发送给 Copilot。此文件应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目简介、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可以为特定任务或文件类型创建，例如为 TypeScript 或 Astro 提供指导，或指导如何创建 UI 组件和单元测试。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级说明以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [也支持其他规范的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论说明文件的编写方式超出本工作坊范围，但示例项目展示了典型做法：

- 在 `copilot-instructions.md` 中集中编写项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件、数据层）或任务提供具体说明。
- 使用自然语言，保持指导清晰，并提供代码应该和不应该如何编写的示例。

说明文件没有唯一的编写方式，AI 的使用方法也不止一种。通过实践，你会找到适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应该拥有完善的说明文件。查看本项目的文件时，可以看到它们涵盖多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成说明文件。不同界面的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前工作流程的课程会在相关位置介绍。
>
> 需要模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含说明文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义说明文件

花些时间阅读此存储库附带的说明文件：一个核心 `copilot-instructions.md` 文件，以及一组面向不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web UI 中打开它们。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。还要查看 **Code standards** 下的 **GitHub Actions Workflows** 子部分。这些内容适用于与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro 文件、Drizzle 数据层、测试等方面的说明。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了一个相对于存储库根目录的 glob，用于决定说明适用的文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件会应用此说明。
5. 查看专门用于为本项目创建单元测试的说明。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。查看指向其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可将较大的说明集拆分为更小、可复用的文件，并为 Copilot 提供代码示例。路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求代码内文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 为更改创建分支

遵循良好的开发实践，为更改创建分支。

1. 返回上一个练习使用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新说明前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始干净的对话。模式和模型选择因练习而异；相关步骤会在需要时说明。

先在当前说明生效的情况下发送提示，以便观察自定义说明的影响。稍后会更新说明并再次发送相同提示。

1. 关闭之前练习中打开的编辑器标签页，确保 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道 helper 应位于何处。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改 helper 文件及其测试。
6. 注意建议的 helper 是带类型的函数，以 `db` 客户端为第一个参数，并返回带类型的 publishers 数组。这来自 `.github/instructions/drizzle.instructions.md` 中的数据层约定，该文件适用于 `src/lib/*.ts`。
7. 注意建议的代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，即使未明确要求，也可能添加文档注释。出现这种情况也没关系；这里的重点仍是观察更新说明后行为的一致性提升。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，该部分应该位于第 27 行附近。它记录了项目编码标准，但尚未说明代码内文档要求，这也是之前生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，说明文件既可以在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 再次运行提示并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空当前对话。
2. 重新打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确文件。
3. 再次发送之前的**相同提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意建议的文件现在会以类似以下注释块的内容开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意建议的函数现在包含类似以下的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你刚才引导 Copilot 遵循新的项目标准，并将标准应用到真实代码中。下一练习将以此为基础继续构建。

## 提交说明并推送分支

说明文件与存储库中的其他资源一样，都应通过源代码管理来维护。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认说明更新和 helper 更改已就绪：

   ```bash
   git status
   ```

3. 暂存并提交说明更新，以及 Copilot 为 helper 基础实现更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示其他配套更改（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并关联积压工作中的文档标准议题。也可以手动操作，但 Copilot 可以使用项目已连接的 GitHub 工具代为处理。请 Copilot 查找议题、创建用于关闭该议题的 PR，然后合并。

> [!NOTE]
> 这是首次让 Copilot 代表你操作 GitHub。项目模板已在工作区配置 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示登录 GitHub。按照提示授权即可。后续练习会介绍这种连接的工作方式（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中已选中 **Agent**。
3. 请 Copilot 查找与更新说明文件相关的议题，并从当前分支创建 PR，说明其中包含说明更新和新的 publishers helper 基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求代表你执行 GitHub 操作时，请先检查命令，再酌情选择 **Allow**。
6. PR 创建后，使用以下提示请 Copilot 合并 PR，并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表你执行 GitHub 操作和 shell 命令时，请先检查命令，再酌情选择 **Allow**。

现在，你已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

你已了解 Copilot 如何读取本项目说明文件中的上下文，并在 VS Code 中使用 Copilot Chat 完成以下事项：

- 发送代码生成提示，观察 Copilot 如何根据*现有*说明生成代码。
- 在 `.github/copilot-instructions.md` 中添加新的全库标准。
- 再次运行相同提示，观察建议的代码如何采用新标准。
- 将说明更新和 helper 基础实现提交到 `main`，供下一练习继续使用。

下一步，Copilot 将在[智能体模式][next-lesson]中使用这些说明，跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义说明][personal-instructions]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
