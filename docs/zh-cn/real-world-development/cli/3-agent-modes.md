---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确定实施方案，使用 Autopilot 根据议题构建筛选功能，并使用 Interactive 模式检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已经为项目添加了一项小功能。更大的更改需要更完善的流程。GitHub Copilot CLI 可以配合组织现有的工作流程，帮助我们以正确的方式构建正确的内容。本课是系列课程的第一课：我们将从议题着手，遵循典型的智能体开发流程来创建新功能、验证代码、确认功能行为，最终将更改合并到项目中。

> [!NOTE]
> 后续课程会继续使用同一对话和分支。实际工作中，通常会为不同文件类型使用不同分支或 PR；本工作坊会简化流程，以便专注于核心概念。

本课将：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

后续课程将继续更新存储库说明、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访客需要按类别和发行商筛选游戏。积压工作中的议题描述了这项功能，但组合类别等细节需要在编码前达成一致。你将使用 Plan 模式确定这些决定，然后授权 Autopilot 在明确范围内实施。

## 背景

将 AI 编码智能体引入开发流程，并不会改变基本原则。事实上，这些原则变得更重要了！大多数开发者遵循的流程大致如下：

1. 从已提交的议题开始，议题描述待完成的工作。
2. 制定构建计划。
3. 编写并检查代码。
4. 运行测试来验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多数流程都与上述步骤类似。

遵循这种标准方法，可以确保 AI 生成的代码符合要求，并经过与手写代码相同的审查流程。

## 对话模式

**对话模式**决定智能体拥有多大自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与智能体协作。智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试，并不断迭代直至完成任务。

先使用 Plan 模式，检查计划，然后使用 Autopilot 实施计划。

## 从议题开始

开始筛选功能的工作前，返回 Codespace 并确保存储库和终端已就绪。

1. 返回 Codespace。如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，并返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划可以让你在将功能或任务交给 Copilot 前，先确定实施方法。对于复杂任务，花些时间规划总是有益的。切换到 Plan 模式，然后请 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会开始制定计划：它会先探索项目，再确定合适的实现方案。

3. 在此过程中，Copilot 可能会询问筛选功能应如何工作。请根据你的偏好回答，这些问题没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动查看计划。
6. 如果计划中的任何部分不符合你的决定，请要求 Copilot 修改。

## 批准 Autopilot

计划已经编写并检查完成，现在可以开始实施了。让 Copilot 使用 Autopilot 执行任务。

Autopilot 会让 Copilot 持续处理问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)** 或当前版本中含义相同的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何按照已确定的计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实施，因此请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决环境问题，再将验证视为已完成。

## 检查并验证实现

生成的代码与其他代码一样，必须在合并前检查。检查代码并运行网站，确保一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff` 并检查筛选功能的实现和测试。
3. 对照议题和规划时作出的决定检查结果。
4. 检查完成后，按 <kbd>Esc</kbd> 退出差异视图。
5. 检查项目验证的输出，并要求 Copilot 修复任何失败项。

## 体验新功能

代码看起来没问题，但它能运行吗？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别筛选、按发行商筛选，以及规划时约定的组合方式。
4. 确认重置和无结果时的行为符合议题和你作出的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同的对话模式构建并检查功能。本课中，你：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现新功能。
- 检查代码。
- 在转发的浏览器中手动验证功能。

下一课将[使用自定义说明][next-lesson]，深入了解如何生成符合文档规范的代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
