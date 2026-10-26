---
title: "练习 1 - 自定义说明（Cloud agent）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot cloud agent →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或 Copilot 需要了解背景信息，就应确保它能访问这些上下文。[说明文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做*什么*，还了解要*如何*完成。

本练习将：

- 了解项目上下文、编码准则和文档标准如何通过存储库自定义说明及按路径限定范围的说明文件传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加新的全库标准。

> [!NOTE]
> 与 VS Code 和 CLI 工作流程不同，本练习不会使用提示进行“前后对比”。Copilot cloud agent 会异步处理 GitHub 议题，因此难以实时并排展示影响。后续练习中审查 cloud agent 创建的拉取请求时，可以看到说明文件的作用。

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

- `.github/copilot-instructions.md`：针对存储库的单个说明文件，每次请求都会发送给 Copilot。此文件应包含适用于大多数请求的项目级信息，例如技术栈、项目概览、最佳实践和其他全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建，例如为 TypeScript 或 Astro 提供指导，或指导如何创建 UI 组件和单元测试。

> [!NOTE]
> 在 IDE 中，说明文件只用于 Copilot Chat 生成代码，不用于代码补全或下一次编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot cloud agent 在生成代码时都会使用存储库级说明以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [也支持其他规范的说明文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理说明文件的最佳实践

完整讨论说明文件的编写方法超出本工作坊范围，但示例项目展示了典型做法：

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
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到底部。查看指向其他说明文件（如 `unit-tests.instructions.md`）和项目现有文件的链接。这种方式可将较大的说明集拆分为更小、可复用的文件，并在生成代码时提供参考示例。这里的路径相对于说明文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档。接下来会添加 TSDoc 文档注释和文件注释头规则。

## 添加新的存储库标准

本练习要进行的唯一编辑，是添加一条全库规则，要求代码中包含 TSDoc 文档注释和文件级注释头。之后本工作流程中分配议题时，cloud agent 会读取该规则。

编辑前，先创建并切换到新分支。Cloud agent 会读取议题目标分支上的说明，但仍建议通过分支提交更改：

1. 在 Codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加存储库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应该位于第 27 行附近。它记录了项目编码标准，但尚未说明代码内文档要求，这也是之前生成的 helper 没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，说明文件既可以在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并说明更新

Cloud agent 会读取议题目标分支上的说明。下一练习中，将议题分配给 Copilot 时，它会从 `main` 创建分支，因此必须先将说明更改合并到 `main`，cloud agent 才能读取。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。下一练习中，cloud agent 处理议题时就会读取这些说明。

> [!TIP]
> 如果你希望在本工作坊中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处使用分支是为了模拟真实项目中的工作流程。

## 总结和后续步骤

你已了解 Copilot 如何读取本项目说明文件中的上下文，并在 `.github/copilot-instructions.md` 中添加新的全库标准。后续工作流程中，你会看到 cloud agent 创建的拉取请求如何应用此标准。

下一步将[把第一个议题分配给 Copilot cloud agent][next-lesson]。

## 资源

- [GitHub Copilot 自定义说明文件][instruction-files]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [为 Copilot 编写更好自定义说明的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
