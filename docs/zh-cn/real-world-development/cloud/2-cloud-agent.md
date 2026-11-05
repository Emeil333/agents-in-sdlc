---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都面临技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期积压的功能请求。GitHub Copilot 云智能体可以自主更新代码和添加功能。完成工作后，它会创建草稿 PR，供开发者审查。这样可以将繁琐任务交给代理，加快开发流程，让开发者专注于更重要的工作。

本练习将介绍如何使用 Copilot 云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务需要处理。最初受雇开发网站的承包商没有留下完善的文档，事实上项目几乎完全缺少文档。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队希望改进游戏发现功能。他们想在每款游戏的详情页显示同类别的其他游戏，方便用户继续浏览。暂时不需要完善的实现，只需要能进行用户体验验收测试的版本。此工作目前受阻，而且还有优先级更高的议题。

这类工作容易被推迟，很适合交给 Copilot 云智能体异步处理。开发者可以继续处理其他任务，之后再返回审查 Copilot 的成果。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发者一样在后台执行任务。可以通过多种方式启动工作，例如[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行期间或完成后查看它的工作。解决方案准备好审查时，Copilot 会在拉取请求中提及你。

## 明确任务范围的重要性

GitHub Copilot 并非魔法工具，使用它并不能省去思考过程。即使看似简单的操作，也可能隐藏复杂问题，因此要[认真界定 Copilot 云智能体的任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段处理、不断学习和试验，并根据结果调整。引入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何引导 Copilot。如果已完成自定义指令练习，就已经见过 `.github/copilot-instructions.md` 的作用；如果还没完成，现在可以快速查看。将任务分配给 Copilot 云智能体前，先只读查看存储库中已有的指令文件，稍后便能观察其影响。

在 GitHub Web 界面或已启动的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：留意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一节将 `Code lacks documentation` 议题分配给云智能体时，注意查看 PR 中的 TSDoc 文档注释、TypeScript 约定和文件注释头。这些要求来自上述指令文件。后续练习还会再次审查这些 PR。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本，以确保项目状态良好。将任务分配给以类似软件工程师方式工作的 Copilot 时也一样。

云智能体使用 [GitHub Actions][github-actions] 执行任务。可以创建[专用设置工作流][setup-workflow]，在 `.github/workflows/copilot-setup-steps.yml` 中配置，以便 Copilot 开始工作前安装所需开发工具和依赖项。实验已预先配置该工作流，确保 Copilot 可以使用 Node.js、项目依赖、端到端测试所需浏览器，以及单个 Astro 应用所用的迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流相似，但有几个要点：

- 它包含名为 `copilot-setup-steps` 的单个作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 触发器允许你从存储库的 **Actions** 选项卡手动运行工作流，以便验证它是否成功，而不必等 Copilot 启动。

## 添加文档

虽然人人都知道文档的重要性，但许多项目的文档已经过时或根本不存在。这类技术债务往往长期无人处理，会降低效率，也会增加维护代码库或引入新团队成员的难度。Copilot 擅长生成文档，因此很适合处理这项工作。它会在后台生成所需文档，之后可以回来审查。

1. 在新浏览器选项卡中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

   ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新建的拉取请求 (PR)，其标题类似 `[WIP]: Code lacks documentation`。如果列表中没有新 PR，请稍等片刻并刷新浏览器。
12. 几分钟后，Copilot 应会创建待办事项列表。

> [!NOTE]
> Copilot 可能需要几分钟才会在 PR 中显示待办事项列表。它正在创建环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看待办事项和计划执行的任务。
14. 向下滚动 PR 时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

   ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新页面才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何探索代码并了解当前状态、暂停思考并制定计划，以及编写代码。

这可能需要几分钟。Copilot 云智能体的一项主要优势是能够异步执行任务，让你腾出时间处理其他工作。现在利用这一点，再分配一项任务，然后开始处理应用程序功能。

## 在游戏详情页添加相关游戏

GitHub Copilot 云智能体的一项优势是可以拆分工作：你处理一组任务时，它可以同时处理另一组。即使为设计团队添加相关游戏区段不需要太多时间，把它委派出去仍能让你专注于其他任务。现在将任务分配给 Copilot 云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

   ![将议题分配给 Copilot](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后你会在议题的第一条评论旁看到一组 👀，表示 Copilot 已开始处理。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。它的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。现在开始探索自定义代理。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体异步处理，用它解决技术债务、创建功能，或协助将代码迁移到其他框架。

你探索了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 编写范围明确的议题。
- 将议题分配给 Copilot。

云智能体正在后台工作，现在可以开始创建并使用自定义代理。[Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可读取前面模块介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义代理 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
