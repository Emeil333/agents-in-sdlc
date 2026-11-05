---
title: "练习 5 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义代理][previous-lesson] |
|:--|

启用 GitHub Copilot 代理模式后，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些工作在本机实时执行，VS Code Copilot Chat 会持续显示每次工具调用和文件编辑。你可以直接在聊天中查看差异、接受或拒绝单项更改，并通过后续提示词引导对话，继续完善或扩展工作，而无需离开编辑器。

本练习将学习如何：

- 实时监控无障碍代理的工作。
- 审查并接受建议的文件编辑。
- 在会话进行期间引导代理，在高对比度模式之外添加浅色模式切换开关。

## 场景

你在[练习 4][previous-lesson]中使用的无障碍自定义代理正在实现高对比度模式。工作进行期间，设计团队又提出新需求：他们也希望为觉得默认主题太暗的用户添加浅色模式切换开关。无需从头开始新对话，可以引导现有会话扩展功能。

## 监控 Copilot 的进度

Copilot Chat 视图会显示代理执行的每个步骤，包括探索的文件、运行的命令和提出的编辑。每项文件编辑都会以差异形式显示，并提供 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中使用的 Copilot Chat 视图。
2. 代理工作时滚动聊天内容，查看每次工具调用。你会看到文件读取、文件编辑以及需要运行的终端命令。
3. 代理提出文件编辑时，聊天中会显示差异：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让代理再次尝试。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看该会话中代理修改的所有文件。

> [!NOTE]
> 如果工具调用要求授权（例如运行终端命令），代理会暂停并显示 **Allow** 提示。只批准你认可的命令。

5. 等待代理完成高对比度模式的实现。完成后应会显示更改摘要。

## 引导代理添加浅色模式

高对比度模式已完成，现在在同一对话中添加浅色模式切换开关。继续使用同一聊天会话时，代理会保留之前工作的上下文，包括修改的组件、使用的样式方案和本地存储模式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

   ```plaintext
   Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
   ```

2. 代理会检查已有更改，并提出浅色模式所需的新增内容。
3. 像审查高对比度更改一样逐项审查新编辑，并使用 **Keep** 或 **Undo**。
4. 如果代理的实现方向不符合预期，例如试图重构切换开关 UI，可以选择聊天输入框旁的 **Stop** 按钮停止工作，再发送澄清提示词。

> [!TIP]
> 与重新开始相比，引导正在进行的会话更快。可以用后续提示词要求调整（例如“简化 CSS，使用 CSS 变量而不是重复选择器”）、纠正方向（例如“不要添加第三个切换开关，将浅色模式整合到现有设置面板”），或扩大范围（例如“为两个切换开关添加键盘快捷键”）。

## 审查工作集

提交工作前，快速检查代理修改的所有内容。

1. 在 VS Code 中打开 **Source Control** 视图。
2. 审查所有更改的文件。你会看到 Astro 组件、样式和相关测试等更新。
3. 打开部分文件查看差异。确认代码体现了无障碍代理的实践，例如 ARIA 属性、键盘导航、语义化 HTML 和本地存储持久化。
4. 确认无误后，在 Source Control 面板中暂存并提交更改。下一课将发布这些更改。

## 总结和后续步骤

你使用 Copilot Chat 代理模式监控并引导正在进行的会话，审查每项文件编辑、接受或拒绝更改，并通过后续提示词扩展工作，同时保留上下文。

本课中，你了解了：

- 如何实时监控代理的工作。
- 如何接受或拒绝单项文件编辑。
- 如何使用后续提示词引导正在进行的会话。

下一课将发布无障碍更改为拉取请求，并审查本地自定义代理在整个实验中的成果。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用代理模式][agent-mode]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代处理 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
