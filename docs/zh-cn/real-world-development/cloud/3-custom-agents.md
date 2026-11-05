---
title: "练习 3 - 自定义代理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义代理？

GitHub Copilot 中的[自定义代理][custom-agents-concept]是针对开发工作流中特定任务或领域定制的 AI 助手。通过在存储库的 `.github/agents` 文件夹中创建 Markdown 文件，可以提供专用指令、最佳实践、编码模式和领域知识，帮助 Copilot 更有效地完成特定类型的工作。团队可以将专业知识整理成可复用代理，例如强制遵循 [WCAG][wcag] 的无障碍代理、遵循安全编码实践的安全代理，或维护一致测试模式的测试代理。

自定义代理由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以全局放在 `~/.copilot/agents` 中。每个文件都包含 YAML frontmatter，至少需要 `name` 和 `description`，后面是定义代理行为、专业知识和指令的 Markdown 提示词。

### 自定义代理与智能体技能的区别

自定义代理和[智能体技能][agent-skills-concept]有些相似：两者主要通过 Markdown 文件定义，都会告诉 AI 如何执行操作。最简单的区分方式是：**自定义代理是执行任务的工作者，技能则是工具**。

自定义代理有独立的上下文窗口，并负责协调技能（甚至其他代理）来完成任务。本实验中，无障碍自定义代理会根据无障碍指南审查并更新网站；它可以在工作中调用拉取请求工作流技能或运行和管理测试的技能。

> [!NOTE]
> 自定义代理没有唯一正确的编写方式。与 AI 的其他用法一样，需要测试和迭代，找到适合环境和场景的方法。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

自定义代理练习将介绍：

- 自定义代理的定义方式。
- 如何将任务分配给自定义代理。

## 场景

Tailspin Toys 致力于让所有用户都能使用其众筹平台，不受视力能力或偏好的影响。近期用户反馈指出，部分用户认为当前暗色主题的文字与背景对比度不足，难以阅读。为解决无障碍问题，设计团队要求增加可供用户切换的高对比度模式。

无障碍至关重要，因此希望尽快实现此功能。接下来使用自定义代理生成实现。

## 查看无障碍自定义代理

项目已提供无障碍自定义代理。查看其内容，了解它如何引导 Copilot。

返回 Codespace，查看无障碍自定义代理文件：

1. 打开 `.github/agents/accessibility.md`。
2. 查看包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义代理必须包含 `name` 和 `description` frontmatter。

3. 继续浏览后面的部分，了解代理职责、无障碍最佳实践、HTML/CSS/JavaScript 代码示例以及常见错误。

## 创建议题并分配任务

Mission Control 是管理环境中所有代理的中心位置。可以将任务分配给 Copilot 云智能体、监控任务，并调整正在进行的会话或提供额外指导。现在将创建高对比度模式的任务并分配给 Copilot。

1. 打开存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue**。
5. 将 **Title** 设置为 `Add high contrast mode to website`。
6. 将 **Description** 设置为：

   ```plaintext
   We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
   ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。
9. 从自定义代理列表中选择 **Accessibility agent**。

   ![云智能体分配界面截图，其中高亮显示自定义代理和无障碍代理](../../../_images/ex5-select-custom-agent.png)

10. 选择 **Assign**。
11. Copilot 会在后台开始处理任务。

## 总结和后续步骤

本课介绍了 GitHub Copilot [自定义代理][custom-agents]，也就是针对特定任务和领域定制的 AI 助手。通过自定义代理，可以将团队专业知识和标准整理成可复用配置，引导 Copilot 更有效地完成特定工作。

你探索了以下内容：

- 自定义代理的定义方式。
- 如何将任务分配给自定义代理。

Copilot 正在实现高对比度模式。接下来，从 Mission Control [监控并引导代理会话][next-lesson]。

## 资源

- [关于自定义代理][custom-agents]
- [在组织中使用自定义代理的准备工作][org-custom-agents]
- [在企业中使用自定义代理的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理代理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
