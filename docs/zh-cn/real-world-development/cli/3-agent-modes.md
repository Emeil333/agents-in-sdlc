---
title: "第 3 课 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方案、使用 Autopilot 根据议题构建筛选功能，并在 Interactive 模式下审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。较大的更改则需要更完善的流程。GitHub Copilot CLI 可以配合组织现有流程，帮助团队以正确的方式构建正确的功能。接下来的几课将介绍典型的代理驱动开发流程：从议题着手创建功能，确保代码有效、行为符合预期，最终成功合并到项目中。

> [!NOTE]
> 后续功能工作将沿用同一个对话和分支。实际工作中，通常会为不同类型的文件创建不同分支或 PR；本研讨会为聚焦核心概念而简化了流程。

本课将学习如何：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续实现此功能时，你还将更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 代理并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的议题描述了该功能，但组合类别等细节需要在编码前确定。你将使用 Plan 模式解决这些问题，然后授权 Autopilot 在明确范围内实现功能。

## 背景

在开发流程中引入 AI 编码代理并不会改变基本原则，反而使这些原则更加重要。多数开发者的流程大致如下：

1. 从描述需求的已创建议题开始。
2. 制定实现计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码通过审查且持续集成流程成功后，合并代码。

> [!NOTE]
> 具体流程因团队和组织而异，但大多数流程都与上述步骤类似。

遵循标准流程可以确保 AI 生成的代码满足需求，并像手写代码一样经过审核。

## 对话模式

**对话模式**决定代理拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：你与代理协作。代理提出更改建议，并等待你的输入后再继续。
- **Plan**：代理先制定计划，但不能编辑项目文件。
- **Autopilot**：代理自主工作，包括编写代码、运行测试并不断迭代，直至任务完成。

先使用 Plan 模式，审查计划，然后使用 Autopilot 实现。

## 从议题开始

开始筛选工作前，返回 Codespace 并确认存储库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

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
9. 按 <kbd>C</kbd> 将议题添加到提示词中，并返回 **Session** 选项卡。

此时提示词开头会显示 `#7`（或类似编号）。在提示词中加入 `#`，即可将 GitHub 议题或拉取请求（PR）添加到上下文。

## 规划筛选功能

规划可以先明确功能实现方法或任务步骤，再交由 Copilot 执行。对于复杂任务，花些时间规划通常很有帮助。切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示词下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示词：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会先探索项目，再确定合适的实现方案。

3. Copilot 可能会询问筛选功能应如何工作。根据自己的偏好回答，没有唯一正确答案。
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 上下滚动审查计划。
6. 如果计划中的任何部分不符合你的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已编写并审查，现在可以开始实现。让 Copilot 使用 Autopilot 执行工作。

Autopilot 会持续处理问题，直至认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或已安装版本中含义相近的选项。
2. 确认提示词下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能立即开始，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

与其他代码一样，生成的代码在合并前也需要审查。审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 将结果与议题及规划时做出的决定进行比较。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 修复任何失败。

## 探索新功能

代码看起来没问题，但运行是否正常？像之前一样启动应用，并通过 Codespaces 转发端口打开网站。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试类别筛选、发行商筛选，以及计划中约定的组合筛选。
4. 确认重置和无结果时的行为符合议题要求和你的决定。
5. 返回 Codespace，让 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你已使用不同对话模式构建并审查功能。本课中，你：

- 从 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式下明确需求。
- 使用 Autopilot 模式实现功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

接下来，通过[使用自定义指令][next-lesson]深入了解如何让生成的代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
