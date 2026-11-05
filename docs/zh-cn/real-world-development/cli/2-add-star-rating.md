---
title: "第 2 课 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中检查更改，并将其作为第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以开始对项目进行第一次更改。先从小处着手：游戏数据中已有星级评分，但主页上的游戏卡片还没有显示它。让代理展示评分、审查更改，并将其作为第一个拉取请求合并。

本课将学习如何：

- 在功能分支上开始专注的 Copilot 对话。
- 让代理对项目进行小幅更改。
- 使用 `/diff` 审查更改。
- 运行应用，在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都有星级评分，详情页已经显示评分，但主页上的游戏卡片只显示标题、类别、发行商和描述。作为热身练习，让代理在每张卡片上显示现有评分。这项更改范围小且独立，非常适合作为第一次会话的任务。

## 对话的组成

**对话**是使用 Copilot CLI 完成任务的工作空间。与 Copilot app 不同，普通 CLI 对话使用终端当前检出的存储库和 Git 分支，而不是创建专用工作树。保存的对话可以让你稍后返回同一讨论，而文件和分支仍是磁盘上的常规 Git 状态。

对话中包含三部分：你的提示词和代理回复、代理探索和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 开始对话并提出更改请求

启动新对话以实现功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未运行，请执行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 开始新会话，然后按 <kbd>Enter</kbd>。
5. 使用以下提示词请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到显示游戏详情所用的文件，并创建所需代码。现在，你已使用 Copilot CLI 添加了一项功能。

## 审查差异

合并之前应审查所有 AI 生成的更改，即使改动很小也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个更改的文件。
2. 确认游戏卡片在有评分时显示数字，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 查看 Copilot 运行的检查结果，并要求它修复任何失败。
5. 审查完成后，按 <kbd>Esc</kbd> 退出差异界面。

> [!NOTE]
> Copilot 和其他生成式 AI 工具一样具有概率性，并非确定性工具，因此生成的代码可能不同。应审查行为，不要期待某一种固定实现。

## 检查更改

不要只看代码就假定它能正常工作。让 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的界面。

1. 让 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示了满分为五分的评分。
4. 返回 Copilot，让它停止刚才启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

功能已完成，现在可以创建拉取请求（PR），将新代码合并到项目中。

1. 让默认代理提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL 以打开 PR。
4. 审查更改的文件和检查结果。
5. 确认无误后，选择 **Merge pull request**，然后确认合并。
6. 返回 Codespace，使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot CLI 发布了第一个更改。具体来说，你：

- 在功能分支上开始了专注的 Copilot 对话。
- 指示代理修改游戏卡片。
- 使用 `/diff` 审查更改。
- 在转发的浏览器中运行应用并确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，从筛选议题开始，使用 Plan 和 Autopilot 模式[构建更大的功能][next-lesson]。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
