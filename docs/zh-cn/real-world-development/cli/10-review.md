---
title: "第 10 课 - 总结与后续步骤"
description: "回顾 Copilot CLI 工作流、两个拉取请求、可复用的自定义配置和更多资源。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

你已在 Tailspin Toys 的连续工作流中使用 GitHub Copilot CLI。本次研讨会中，你：

- 准备 Codespace、安装 Copilot CLI、探索项目并找到预先创建的筛选议题。
- 添加星级评分，在转发的浏览器中检查结果，并手动合并第一个拉取请求 (PR)。
- 从筛选议题开始，在 Plan 模式下确定方案，在 Autopilot 模式下实现，并在 Interactive 模式下审查。
- 使用自定义指令引导代理，然后自定义现有的 `quality-checks` 技能，用它运行单元测试、lint 和类型检查。
- 添加 Playwright Model Context Protocol (MCP) 服务器，并在真实浏览器中探索筛选功能。
- 创建并选择质量保证 (QA) 自定义代理，以评估需求、覆盖率、技能结果和浏览器证据。
- 审查完整的筛选更改，并授权 Agent Merge 合并筛选 PR。
- 探索用于管理上下文、模型、共享和可选云端委派的斜杠命令。

## 交付的内容

本研讨会有两个 PR 阶段，每项更改都基于更新后的 `main` 分支：

1. **星级评分**：在游戏卡片上显示现有的 `starRating` 和明确的未评分状态。
2. **筛选和质量工作流**：实现筛选、更新指令并将其应用到功能、自定义 `quality-checks` 报告、创建 QA 配置，并加入相关测试。

从规划筛选功能到创建 PR，你一直使用同一个对话和分支。为了简化研讨会流程，我们将这些工作合并到一个 PR 中。

## 不同类型的验证

你通过自动化测试、浏览器手动检查以及 Copilot 使用 MCP 探索浏览器，采用多种方式检查代码。`quality-checks` 技能运行单元测试、lint 和类型检查，并按新格式报告结果。QA 将这些结果与需求和测试覆盖率审查整合到一起，再供 PR 使用。

新增测试应解决实际覆盖不足。如果 QA 确认无需新增测试，也可能是合理结果。授权合并之前，请审查代码和证据；更改后应更新受影响的验证证据。

## 最佳实践

提供给 Copilot 的上下文和工具会影响它的工作。在本研讨会中，你更新了指令、自定义技能、创建 QA 配置并配置 MCP 服务器。可以在多个对话中复用这些自定义配置，并根据团队需求调整。指令设定标准，技能描述可重复任务，自定义代理定义专用角色，MCP 服务器连接外部工具。审查实际更改和工具结果，而不只是代理的摘要。

根据任务选择合适的**模式和模型**。使用 **Plan** 先考虑方案，使用 **Interactive** 参与专注的更改，使用 **Autopilot** 处理范围明确的任务。例行编辑可选择更快的模型，复杂工作则可选择能力更强的模型。

上下文仍与基础设施同样重要。清晰说明要构建什么、为什么构建以及如何实现，会显著影响结果。

## 更多可探索内容

你已完成核心工作流。还可以了解一些 Copilot CLI 功能：

- 使用 `/review` 让代码审查代理分析更改。
- 使用 `/rubber-duck` 讨论问题并获得另一种观点。
- 使用 `/fleet` 并行协调互相独立的子任务。
- 使用 `/worktree` 隔离单独任务。
- 使用 `/delegate` 将任务委派给 Copilot 云智能体。

## 后续步骤

提升工具使用能力的最佳方法是继续实践！可以用它处理生产代码、兴趣项目，或者多年来一直想构建却尚未着手的小应用。与团队分享经验，也向团队成员学习。最后，继续探索文档。

如果要从终端扩展 Tailspin Toys，可以继续学习可选的 [Foundry Backer Concierge 系列][foundry]。若要比较其他环境，还可以探索 [VS Code 研讨会][vscode]、[GitHub Copilot app 研讨会][app]或 [Copilot 云智能体研讨会][cloud]。

## 资源

- [关于 GitHub Copilot CLI][about-cli]
- [Copilot CLI 命令参考][cli-reference]
- [自定义 Copilot CLI][customize-cli]
- [使用 Copilot CLI 管理拉取请求][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
