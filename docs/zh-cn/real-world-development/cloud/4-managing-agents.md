---
title: "练习 4 - 监控和管理代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几个练习中，你让 Copilot 云智能体执行了三项改进用户体验和添加功能的任务。云智能体可以异步自主工作，但仍需监控任务。

可以通过多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[代理页面][agents-page]。在这个控制中心，可以查看所有创建了开放拉取请求 (PR) 的代理任务、查看它们执行的操作，并引导正在进行的会话。

本课将学习如何：

- 使用代理页面监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

## 场景

将任务分配给代理以创建高对比度模式后，团队意识到也可以同时添加浅色模式。由于网站样式和切换功能正在更新，加入这项功能很合理。现在引导代理同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务状态。

1. 打开 [Copilot 代理页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。列表中应包含你分配给 Copilot 的任务，例如：
   - 为代码库补充文档。
   - 为游戏生成修改 API。
   - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 执行的操作，例如检出存储库代码、创建工作环境、设置 MCP 服务器以及完成任务所需的各项步骤。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和执行方式而变化。

4. 留意右侧的拉取请求 (PR) 窗格。在此可以查看 PR 和更改的文件，以便进一步监控。

## 引导云智能体

了解正在运行的任务后，让 Copilot 在实现高对比度模式的同时也加入浅色模式切换开关。

1. 选择与高对比度模式相关的会话。会话标题因 Copilot 采用的名称和当前工作状态而异。

   ![Mission Control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到代理完成设置并开始工作。此时会出现类似下方的消息。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示词：

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![代理页面中的云智能体任务截图，其中高亮显示 steer active session while copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 留意 Copilot 如何确认提示词，并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。收到消息时，它会先完成当前步骤，再将新请求加入工作流程。

这通常需要几分钟，可以趁此机会回顾迄今为止学到的内容。

## 总结和后续步骤

本课介绍了 Copilot 代理页面，这是监控和引导 GitHub Copilot 云智能体任务的中心位置。通过 Mission Control，可以跟踪所有进行中和已完成的任务、审查工作内容，并调整进行中的任务范围或提供额外指导。

你探索了以下内容：

- 使用 Mission Control 和代理页面监控云智能体任务。
- 引导正在进行的会话，请求添加其他功能。

Copilot 正在完成无障碍功能，接下来可以[迭代改进它创建的拉取请求][next-lesson]。

## 资源

- [Copilot 代理页面][agents-page]
- [关于自定义代理][custom-agents]

---

| [← 上一课：自定义代理][previous-lesson] | [下一课：迭代处理 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
