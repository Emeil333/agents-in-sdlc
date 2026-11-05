---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理代理][previous-lesson] |
|:--|

## 审查工作

在本实验中，你与 GitHub Copilot 协作完成了多项改进用户体验和添加功能的任务：为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现高对比度和浅色模式切换等无障碍功能。现在查看代码更改，必要时向 Copilot 提供反馈以改进结果。

### 场景

如前所述，引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。始终需要审查生成的代码，并遵循正常的 DevOps 流程。接下来审查 GitHub Copilot 生成的文档、相关游戏功能和无障碍功能，然后再启用团队其他成员的审查。

## GitHub Copilot 云智能体与安全

Copilot 云智能体会异步且无人监督地执行任务，因此必须遵守一些安全限制：

- Copilot 仅对存储库拥有读取权限，并且**只能**写入其代码分支。
- 云智能体在 GitHub Actions 中运行，并创建独立的临时环境进行工作。
- 运行任何 GitHub Actions 工作流前，都需要人工批准。
- 默认情况下，[对外部资源的访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)，其中包含为代码添加的文档。使用 GitHub.com 上的标准 PR 界面审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如有需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开 PR 列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。此时请等待 Copilot 完成，可能需要几分钟。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

   ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新增的 TSDoc 文档注释和其他文档。具体更改可能不同。审查时，留意 TSDoc 文档注释、TypeScript 约定和文件注释头；这些要求来自练习 2 开始时查看的自定义指令文件。
6. 审查完成且确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。正常情况下，单个 Astro 应用的项目检查应全部通过。此过程可能需要几分钟。

## 向 GitHub Copilot 请求更改

与团队成员一样，你可以在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取评论，并启动新的会话来处理请求。由于结果具有非确定性，无法给出固定提示词，但可以要求 Copilot：

- 为每个代码文件顶部添加简要说明其用途的注释头。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建介绍 Astro 应用结构的 README。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。可以使用上面的建议，或提出其他代码文档需求。
2. 选择 **View Session**，观察 Copilot 如何开始新会话并进行更改。
3. 选择 **Back to pull request** 返回 PR。

   ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新提交。
5. 选择 **Files changed** 选项卡并审查更改。

可以继续迭代，直至结果满意。之后可将 PR 从草稿状态改为就绪，并合并到 main 分支。

![将 PR 转为就绪状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 审查完成且确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流指示器。
7. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

8. 工作流会在 PR 的检查区域中排队运行。正常情况下，单个 Astro 应用的项目检查应全部通过。此过程可能需要几分钟。
9. **可选：**可以在 Codespace 中检出此分支，手动测试相关游戏功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**：

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot 已构建相关游戏功能。与之前一样，可以通过迭代向云智能体提出修改请求，例如调整显示的相关游戏数量，或要求添加文件注释头和 TSDoc 文档注释（注意：此议题是在更新自定义指令**之前**分配的）。在 **Conversation** 选项卡中添加评论，Copilot 会读取并启动新会话。

## 审查无障碍功能

最后，审查使用无障碍自定义代理实现的功能。PR 应同时包含练习 3 中分配的高对比度模式，以及练习 4 中在 Mission Control 请求的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会包含 **[WIP]**。请等待 Copilot 完成，这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 检查实现，特别关注：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。
6. 审查完成且确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流指示器。
8. 如果有等待批准的工作流，选择 **Approve and run workflows**。

   ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域中排队运行。正常情况下，单个 Astro 应用的项目检查应全部通过。此过程可能需要几分钟。
10. **可选：**可以在 Codespace 中检出此分支，手动测试无障碍功能。打开终端并运行以下命令，将 `<branch-name>` 替换为 Copilot 创建的分支名称：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，并确认设置在页面重新加载后仍然保留。

无障碍自定义代理帮助 Copilot 根据无障碍最佳实践实现这些功能。如果发现无障碍问题或改进点，可以像处理其他 PR 一样在评论中提及 **@copilot**，请求更新。

## 可选练习 - 继续委派

云智能体适合处理真实的待办事项，让你可以转而处理其他工作。可以在自己的存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。本系列练习中，你：

- **检查了存储库自带的自定义指令文件**，以便之后观察它们对云智能体输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、制定计划并异步执行任务。
- **创建并使用自定义代理**处理无障碍工作，添加高对比度和浅色模式切换开关。
- **使用 Copilot 代理页面作为 Mission Control**，监控并引导进行中的无障碍会话。
- **审查并迭代改进云智能体的拉取请求**，提及 `@copilot` 请求更改并批准工作流。

## 回顾和后续步骤

云智能体学习路径已完成。其他学习路径可以补充本次实践内容：

- 🖥️ **[VS Code 学习路径](../../vscode/)**：直接在 IDE 中探索 Copilot Chat 代理模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)**：在终端中交付两项经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 代理、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体工作流。

也可以在自己的存储库中尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义代理。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 代理][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理代理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
