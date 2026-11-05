---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码还涉及数据、外部服务，以及让 Copilot 使用更多自动化工具。这正是 MCP 服务器的用途。MCP 服务器可以让 Copilot CLI 使用内置能力之外的工具和服务。

本课将学习如何：

- 了解 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 在尚未配置时添加 Playwright MCP 服务器。
- 让代理操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际与界面交互。让 Copilot 像用户一样操作网站，可以进一步自动验证更改是否符合预期。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 为 AI 代理与外部工具和服务通信提供了方式。通过 MCP，代理可以实时访问最新信息并代表你执行操作。

代理通过 MCP 服务器访问工具和资源。MCP 服务器充当代理与外部工具、服务之间的桥梁，每个服务器都会提供一组可供代理访问的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化能力。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，帮助发现并参与生态系统。

> [!CAUTION]
> 应像对待项目中的其他依赖项一样对待 MCP 服务器。使用前先审查其源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，让 Copilot 能像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下命令：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能正常。现在使用 Copilot 中配置的 Playwright MCP 服务器完成同样的检查。

1. 使用以下提示词，让 Copilot 通过 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常 Copilot 会自行判断是否使用 MCP 服务器，不一定需要明确指定。不过，说明预期工具可以帮助它更稳定地完成任务，也能节省一些 token。

2. 观察 Copilot 在浏览器中执行的步骤。
3. 阅读报告，确认功能行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并提供报告。

## 总结和后续步骤

你已使用 Playwright MCP 服务器，让 Copilot CLI 在真实浏览器中探索功能。本课中，你：

- 了解了 Model Context Protocol (MCP) 以及 Copilot CLI 如何使用它。
- 在尚未配置时添加了 Playwright MCP 服务器。
- 让代理操作浏览器并探索筛选功能。

接下来，[创建 QA 自定义代理][next-lesson]，将技能与浏览器工具结合起来。

## 资源

- [MCP 是什么？为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
