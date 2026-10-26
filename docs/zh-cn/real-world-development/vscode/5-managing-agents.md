---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

启用 GitHub Copilot 智能体模式后，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作会在你的计算机上实时进行，VS Code 中的 Copilot Chat 会持续显示每次工具调用和文件编辑。你可以内联审查每个差异、接受或拒绝单项更改，还可以使用后续提示引导对话，在不离开编辑器的情况下完善或扩展工作。

本练习将：

- 实时监控无障碍智能体的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导智能体添加浅色模式切换功能。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了另一项请求：他们也希望添加浅色模式切换功能，供觉得默认主题太暗的用户使用。不必从头开始新对话，可以引导现有会话扩展工作范围。

## 监控 Copilot 的进度

Copilot Chat 视图会显示智能体执行的每个步骤，包括探索的文件、运行的命令和建议的编辑。每项文件编辑都会在聊天中内联显示差异以及 **Keep** / **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 智能体工作时，在聊天中滚动查看每次工具调用。你会看到文件读取、文件编辑以及它请求运行的终端命令。
3. 智能体建议编辑文件时，差异会内联显示：
    - 选择 **Keep** 接受更改。
    - 选择 **Undo** 撤销更改，让智能体再次尝试。
4. 也可以选择聊天顶部的 **Files changed** 指示器，查看智能体在此会话中修改的所有文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待智能体完成高对比度模式实现。完成后应该会显示总结更改的消息。

## 引导智能体添加浅色模式

高对比度模式完成后，将继续在同一对话中添加浅色模式切换功能。由于继续使用同一个聊天会话，智能体会保留之前工作的所有上下文，包括修改的组件、采用的样式方法和设置的本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会继续处理之前的工作，探索已有更改并提出浅色模式实现方案。
3. 与高对比度模式一样，逐项检查新编辑，并使用 **Keep** 或 **Undo** 处理各项差异。
4. 如果智能体的实现方向不符合预期，例如尝试以不喜欢的方式重构切换 UI，可以使用聊天输入框旁的 **Stop** 按钮停止它，再发送说明性后续提示。

> [!TIP]
> 引导正在运行的会话比从头开始更快。可以使用后续提示调整工作，例如“简化 CSS，使用 CSS 变量而不是重复选择器”、纠正方向（“不要添加第三个切换控件，将浅色模式整合到现有设置面板”），或扩展范围（“为两个切换控件添加键盘快捷键”）。

## 检查工作集

提交工作前，快速检查智能体修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 检查已更改文件的完整列表。应该能看到 Astro 组件、样式和相关测试的更新。
3. 打开几个已更改文件并检查差异。确认体现了自定义智能体的无障碍模式，例如 ARIA 属性、键盘导航、语义化 HTML 和本地存储。
4. 确认满意后，在 Source Control 面板中暂存并提交更改。下一课会发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 智能体模式监控并引导正在运行的会话：逐项审查文件编辑、接受或拒绝更改，并发送后续提示扩展工作，同时保留已有上下文。

本课中，你了解了：

- 如何实时监控智能体的工作。
- 如何接受或拒绝单项文件编辑。
- 如何通过后续提示引导正在运行的会话。

下一课将[发布无障碍功能并审查 Copilot 的工作][next-lesson]。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
