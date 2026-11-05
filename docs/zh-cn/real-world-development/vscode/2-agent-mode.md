---
title: "练习 2 - 使用 Copilot 代理模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也涉及多个文件以及运行测试等操作。开发者需要找到相关文件、进行修改、运行测试、调试、找出遗漏文件并再次修改，工作往往会不断重复。

Copilot 代理模式可以帮助处理这类工作。

Copilot 代理模式可在 IDE 中更自主地工作。它会先探索现有项目结构，执行必要的更新，运行测试等任务，并自动修复发现的问题。现在了解如何使用代理模式为网站添加功能。

> [!NOTE]
> 代理模式和云智能体名称相似，但适用于不同的工作方式。代理模式在 IDE 中执行任务，便于快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将学习如何：

- 使用 Copilot 代理模式探索项目、查找相关文件并协调多文件更改。
- 使用代理模式跨 UI 和数据层实现新功能。
- 在合并到代码库之前审查 Copilot 生成的更改和测试。

## 场景

游戏列表不断增长，你希望用户能够按类别和发行商筛选。上一练习已添加发行商辅助函数，现在将使用 Copilot 代理模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改网站之前，先了解当前功能。

Tailspin Toys 是一个开发者主题棋盘游戏众筹平台，可列出游戏并显示详细信息。网站是单个 Astro 应用，会在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问辅助函数放在 `src/lib/` 中，交互功能使用小型、限定范围的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 命令，用于启动单个 Astro 开发服务器。按照以下步骤在 GitHub Codespace 中运行网站：

1. 返回 Codespace。上一练习结束后应已切回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到筛选工作分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器启动后，会显示类似以下内容的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321`，请选择 **x** 将其关闭。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 使用 codespace 时，从终端选择 localhost 链接会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是连接到托管 Web 服务器的 codespace 的私有隧道。

### 浏览网站

网站启动后，可以浏览其功能：

- **主页**：显示游戏列表，包括标题、图片和描述。
- **游戏详情页**：选择游戏后会打开详情页，展示标题、描述、发行商和类别等信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相应步骤会在需要时说明。

网站的初始实现已经可以运行，但还需要增加功能。先查看待办事项。使用模板设置项目时，系统已自动创建一组议题；让 GitHub Copilot 显示这些议题。

1. 在 Chat 视图的代理下拉菜单中选择 **Agent**。**Agent** 代理可以自主规划并跨文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写研讨会时使用了 Claude Sonnet 4.5，因此这里列出该模型，希望获得相对一致的体验。但由于大型语言模型具有概率性，收到的建议可能与文档中的示例不同，这是正常现象。

> [!NOTE]
> 由于大型语言模型具有概率性，Copilot 可能使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示词，询问待办事项中的议题：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看生成的议题列表。

Copilot 还会根据它认为对用户最有用的程度，为议题排序。

## 审查指令文件

让代理生成代码前，先查看将用于提供上下文的指令文件。项目中的[用户界面 (UI) 指令][ui-instructions]说明了添加网站功能时应采用的方法。

1. 在 Codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看整体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍原则。
   - 指向各类文件专用指令的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以引用其他指令文件和项目中的文件。路径相对于指令文件所在位置。这种方式便于复用、拆分较大的指令集，并提供示例和模板。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md

## 实现筛选功能

要完成筛选，至少需要更新三个部分：

- 在数据层 (`src/lib/`) 中添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页以加入筛选 UI。

在合并到代码库之前，还需要运行并通过测试。Copilot 代理模式可以执行这些任务。

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图代理下拉菜单中仍选中 **Agent**。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选中 **Claude Sonnet 4.5** 模型。
5. 使用以下提示词，根据待办事项中相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 探索项目并查找相关文件。它会定位数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会探索项目、修改文件并运行测试。根据任务和代码库的复杂程度，这可能需要几分钟。过程中，代码编辑器可能会显示 **Keep** 和 **Undo** 按钮。工作完成后，每项更改都会提供 **Keep** 或 **Undo** 选项，因此无需在工作进行时立即选择。

7. Copilot 提示运行测试时，选择 **Continue**。

   ![Copilot Chat 窗格中询问是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 工作过程中可能会暂停，甚至出现测试失败。这是正常的。Copilot 会在代码生成和测试之间反复迭代，直至任务完成且未发现错误。

   ![Copilot 代理模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 始终需要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行网站的浏览器，探索新功能。
11. 确认功能正常且已审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

本练习介绍了如何使用 GitHub Copilot 代理模式为 Tailspin Toys 网站添加功能。你了解到如何：

- 使用代理模式跨 UI 和数据层实现新功能。
- 让 Copilot 代理模式探索项目、查找相关文件并协调多文件更改。
- 在合并到代码库前审查 Copilot 代理模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 - 实现分页

游戏列表不断增长，网站将需要分页功能。运用本练习学到的技能，提示 Copilot 为网站添加分页。可以考虑：

- 遵循现有最佳实践，包括使用已有指令文件。
- 确定分页方式，例如是否允许用户选择每页大小，还是使用固定值。
- 编写提示词时，为 Copilot 提供实现所需的指导。
- 可能需要通过提供更改请求和上下文不断迭代。这是使用 Copilot 时的正常流程。

## 资源

- [Copilot 的 ask、edit 和 agent 模式：功能及适用场景][choose-mode]
- [VS Code 中的代理模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
