---
title: "第 7 课 - 创建并使用 QA 代理"
description: "创建 QA 自定义代理，将议题需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选功能。现在将这些能力整合到一个定义清晰的 QA 流程中。

本课将学习如何：

- 了解自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置。
- 选择 QA 代理，并根据筛选议题审查其结果。

## 场景

Tailspin Toys 希望在打开拉取请求（PR）前，一致地审查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程，并生成可复用报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明代理的用途、指令和可用工具。本研讨会将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

各类自定义功能各有用途：存储库指令说明团队标准；`quality-checks` 技能封装可重复检查；Playwright MCP 提供浏览器工具；QA 配置则指示 Copilot 如何使用这些能力评估需求并报告结果。QA 配置不会取代这些工具，也不需要另开对话。

## 创建 QA 配置

打开功能 PR 之前，让 Copilot 创建可复用的 QA 配置。配置中会定义 QA 执行的检查以及必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 让默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置

使用新代理前，先确认 Copilot 捕获了预期的 QA 流程和边界，避免配置不完整或授权范围过宽。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 可选，但填写后代理会有清晰的显示名称。
3. 阅读配置指令，确认 QA 会从需求开始，遵循存储库指令，运行 `quality-checks` 技能，并使用 Playwright MCP。
4. 确认 QA 会提供支持证据、修改实现代码前会先询问，而且不会提交更改或打开 PR。
5. 如果生成的配置缺少上述职责或边界，请让默认代理修改后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目代理。创建配置后，恢复同一个筛选对话并选择 QA，以便它使用已有的议题和规划决定。

1. 使用以下命令启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的代理可能暂时不会显示在可用代理列表中，但仍可使用上述命令启用。

2. 使用以下提示词，让 QA 代理审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 代理完成工作。
4. 阅读代理提供的报告。

## 总结和后续步骤

你已添加可复用的专用代理角色并审查了它的工作。本课中，你：

- 了解了自定义代理如何配合指令、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置。
- 选择 QA 代理，并根据筛选议题审查了结果。

现在，功能实现、技能更新、QA 配置、测试和验证报告均已准备好等待审查。接下来，将它们整合到功能 PR 中，并使用 Agent Merge 完成合并。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
