---
title: "第 4 课 - 使用自定义说明指导 Copilot"
description: "了解存储库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应向 Copilot 提供相应指导。[说明文件][instruction-files]不仅描述想要什么代码，也说明代码应如何组织。现在筛选功能已经完成，你将了解 Copilot 使用的说明、添加文档标准，并将其应用于代码。

本课将：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

## 场景

与优秀的开发团队一样，Tailspin Toys 也有一套开发准则和要求，包括：

- 注释应说明意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果函数包含可注入的 `db` 参数，也应记录。
- 可复用的 Astro 组件应说明其 `Props` 约定；相关代码更改时，注释也应保持最新。
- 保留现有格式和 lint 指引。

借助说明文件，可以确保 Copilot 掌握正确的信息，并按照上述实践完成任务。

## 说明文件

自定义说明可以为 Copilot 提供上下文和偏好，帮助它理解编码风格和需求。通过说明文件，可以引导 Copilot 提供更相关的建议和代码片段。你可以指定编码约定、库，甚至希望包含的注释类型。说明既可以针对整个存储库，也可以针对特定文件类型，为任务提供上下文。

说明文件分为两类：

- `.github/copilot-instructions.md`：针对存储库的单个说明文件，每次请求都会发送给 Copilot。此文件应包含适用于大多数请求的项目级信息。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指导的文件。

> [!NOTE]
> 其他说明格式及其支持情况会因环境而异。使用特定格式前，请查阅[自定义说明支持参考][custom-instructions-support]。

## 查看本项目中的自定义说明文件

起始项目已包含一组说明文件。先查看现有文件，再进行更改并观察效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览项目简介和编码指导。此文件适用于在此存储库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。注意其中包含 Astro 文件、Drizzle 数据层、测试等方面的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段，它设置了一个 glob，用于决定说明适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向的其他说明文件和项目文件。这种方式可将较大的说明集拆分为更小、可复用的文件，并为 Copilot 提供参考示例。

## 更新说明文件以符合团队指导

现有说明已经提供了良好基础，但仍有不足。更新核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分大约位于第 35 行。
2. 在该部分最后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的指导

Copilot CLI 会在对话开始时加载存储库说明。保存更新后的说明后，退出并恢复筛选对话，以便 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭它。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并检查已更改的 TypeScript 文件。查看新生成的 TSDoc 注释，并确认其准确说明代码。

## 总结和后续步骤

你已了解 Copilot CLI 如何从项目说明文件获取上下文，并应用了新的编码标准。本课中，你：

- 了解存储库说明和按路径限定范围的说明文件如何传递给智能体。
- 更新说明文件，确保遵循编码标准。
- 观察说明文件对代码的影响。

接下来将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保测试和 lint 检查能够一致地运行。

## 资源

- [为 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [创建自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
