---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：代理模式][previous-lesson] · [下一课：自定义代理 →][next-lesson]

你刚刚使用代理模式构建了筛选功能。创建拉取请求之前，应确认它在浏览器中正常运行。此处将连接 **Playwright MCP 服务器**，让 Copilot 操作真实浏览器测试功能，然后发布分支并创建 PR。

本练习将学习如何：

- 了解 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 工具。
- 将 Playwright MCP 服务器添加到工作区。
- 让 Copilot 使用服务器在浏览器中手动测试筛选功能。
- 发布分支，并为筛选功能创建拉取请求。

## 什么是 Model Context Protocol (MCP)？

代理模式连接编辑器以外的工具后会更加强大。Copilot 使用 Model Context Protocol (MCP) 与外部工具和服务通信。

![代理模式的工作原理示意图，展示它与上下文、LLM 和工具的交互，包括 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) 为 AI 代理与外部工具和服务通信提供了方式。代理可以通过 MCP 实时获取最新信息（使用资源），并代表你执行操作（使用工具）。

代理通过 MCP 服务器访问工具和资源。MCP 服务器负责协调代理与外部工具（例如现有 API 或 npm 包等本地工具）之间的通信。每个 MCP 服务器都会提供一组可供代理访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供一组用于管理 GitHub 存储库的 API，代理可以借此创建和更新存储库、管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化能力，代理可以浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器提供不同的工具和资源。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，帮助发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前请仔细审查源代码、验证发布者并考虑安全影响。只使用信任的 MCP 服务器，谨慎授予其访问敏感资源或执行操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件用于配置 VS Code 工作区中可用的 MCP 服务器。

1. 在 Codespace 中打开 `.vscode/mcp.json`。
2. 你会看到已配置的 `github` 服务器：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

项目模板自带此 `github` 配置，因此 Copilot 能在上一练习中读取议题列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用其中的工具时需要使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

接下来添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可控的浏览器，用于测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会在你的计算机上运行 `npx @playwright/mcp@latest` 来启动它。`--headless` 标记让 Playwright 在后台运行浏览器，不显示窗口。Codespace 中没有桌面环境，因此需要此选项。

> [!NOTE]
> Tailspin Toys 项目已使用 Playwright 进行端到端测试，因此通常已安装 Playwright 所需的浏览器。如果 Copilot 后续报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会按需启动 MCP 服务器。Copilot 首次需要服务器工具时，VS Code 会启动服务器并询问你是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它以立即启动服务器，确认配置正确。
2. 如果 VS Code 询问是否信任服务器，请先审查配置，再选择信任以启动服务器。

手动启动是可选的。如果跳过此步骤，VS Code 会在下一步首次需要服务器工具时启动服务器，并询问是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以让 Copilot 操作真实浏览器。代理可以打开网页、导航、应用筛选并读取结果，然后向你总结观察结果，无需离开对话即可快速验证功能行为。

Playwright MCP 服务器会读取页面的[无障碍树][playwright-mcp-server]，而不是截图。因此，代理会基于带标签的结构化元素（按钮、链接和列表项）进行推理，与辅助技术的工作方式相同。这意味着快速功能检查也能顺便验证一些基础无障碍要求。

连接服务器并启动应用后，使用以下提示词让 Copilot 测试筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器，按照步骤操作并报告结果。根据议题中的验收标准审查报告；如果发现异常，请进一步询问或让 Copilot 修改代码，然后再创建 PR。

> [!NOTE]
> 测试时，应用必须在 `http://localhost:4321` 运行。如果已停止开发服务器，请在发送提示词前重新启动。Copilot 首次使用 Playwright MCP 服务器时，可能需要下载浏览器。如果提示缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。先发布 `filtering-vscode` 分支。

1. 打开 Codespace 中的 **Source Control** 面板，审查 Copilot 所做的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到存储库。

## 创建拉取请求

可以通过 github.com 或 GitHub 命令行界面 (CLI) 创建拉取请求。也可以让 Copilot 查找相关议题并创建关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，开始新会话。
2. 确认代理下拉菜单中选中 **Agent**，以便 Copilot 使用 GitHub 工具。
3. 让 Copilot 创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 收集信息并执行操作。首次使用 GitHub 工具时，系统可能会要求进行 GitHub 身份验证；按提示授权即可。
5. Copilot 会搜索议题、找到正确议题并创建 PR。
6. 选择 Copilot 生成的链接审查拉取请求，但**暂时不要合并**。

## 总结和后续步骤

本练习中，你：

- 了解了 Model Context Protocol (MCP) 以及 MCP 服务器如何扩展 Copilot 工具。
- 将 Playwright MCP 服务器添加到工作区。
- 在发布前使用该服务器在浏览器中手动测试筛选功能。
- 发布了分支，并为筛选功能创建了拉取请求。

你已使用 MCP 服务器测试功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义代理][next-lesson]，简化无障碍审查等专门任务。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
