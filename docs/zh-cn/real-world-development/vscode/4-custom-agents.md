---
title: "练习 4 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件定义代理，可以为 Copilot 提供专用指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定类型的工作。团队可以将专业知识整理成可复用代理，例如强制遵循 [WCAG][wcag] 的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面是定义代理行为、专业知识和指令的 Markdown 提示词。

### 自定义代理与智能体技能的区别

自定义代理和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是执行任务的工作者，技能则是工具**。

自定义代理有独立的上下文窗口，并负责协调技能（甚至其他代理）来完成任务。本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；它可以在工作中调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一正确的编写方式。与 AI 的其他用法一样，需要测试和迭代，找到适合环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，部分用户认为当前暗色主题的文字与背景对比度不足，难以阅读。为解决无障碍问题，设计团队要求增加可供用户切换的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。接下来使用自定义代理生成实现。

本练习将学习如何：

- 查看现有的无障碍自定义代理。
- 在 Copilot Chat 中使用无障碍代理实现高对比度模式。

## 查看无障碍自定义代理

项目已提供无障碍自定义代理。返回 Codespace，打开终端，并从 `main` 新建分支用于无障碍工作，将它与练习 3 的筛选 PR 分开：

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 继续查看后面的各个部分，其中介绍：
   - 生成无障碍网站代码时的核心职责。
   - 无障碍最佳实践。
   - HTML、CSS 和 JavaScript 代码示例。
   - 常见陷阱和错误。

## 在 Copilot Chat 中使用自定义代理

VS Code 会在 Copilot Chat 视图底部的代理下拉菜单中显示 `.github/agents` 中定义的所有自定义代理。选择自定义代理后，聊天会使用该代理的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相应步骤会在需要时说明。

1. 如果代理下拉菜单尚未选择 **Agent**，请先选择它。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部的代理下拉菜单（默认活动代理显示为 **default**）。
3. 从可用代理列表中选择 **Accessibility agent**。
4. 向无障碍代理发送以下提示词：

   ```plaintext
   Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
   ```

5. Copilot Chat 会探索代码库、提出更改并编辑项目文件。每次编辑都会在聊天中以文件路径和差异的形式显示，供你审查。
6. 随着编辑发生，聊天顶部的 **Files changed** 指示器会更新，显示代理修改的文件。

> [!NOTE]
> 这个过程可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等现有文件。

下一练习将介绍如何审查并引导这项正在进行的工作。

## 总结和后续步骤

本课介绍了 GitHub Copilot [自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。通过自定义代理，可以将团队专业知识和标准整理成可复用配置，引导 Copilot 更有效地完成特定工作。

你探索了以下内容：

- 自定义代理的定义方式。
- 在 Copilot Chat 代理模式中使用自定义代理。

接下来，[监控并引导代理的工作][next-lesson]，审查其更改，并在同一会话中添加浅色模式切换开关。

## 资源

- [关于自定义代理][custom-agents]
- [在 IDE 中创建自定义代理][creating-custom-agents-ide]
- [VS Code 中的自定义代理][custom-agents-vscode]
- [自定义代理配置][custom-agents-config]
- [awesome-copilot 上的自定义代理][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
