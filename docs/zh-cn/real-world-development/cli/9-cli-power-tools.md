---
title: "第 9 课 - GitHub Copilot CLI 斜杠命令"
description: "探索用于管理上下文、选择模型、共享会话和可选云端委派的斜杠命令。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

与其他优秀的 CLI 工具一样，GitHub Copilot CLI 提供了许多交互用的斜杠命令。这些命令用于访问高级功能、后台信息和其他配置选项。你已使用过 `/diff`、`/mcp`、`/skills`、`/agent` 和 `/pr`。现在探索其他实用命令。

本课将学习如何：

- 使用 `/context` 和 `/compact` 查看 Copilot 如何管理对话上下文。
- 使用 `/model` 查看可用模型。
- 了解如何使用 `/share` 导出或共享会话。
- 探索并行工作、工作树和委派给云智能体等可选命令。

## 场景

核心 CLI 工作流已完成。现在了解其他能力，包括管理上下文、切换模型、共享会话，以及可选地将工作委派给 [Copilot 云智能体][about-cloud-agent]。

## 探索 Copilot CLI 上下文

处理大型或复杂任务时，可能会接近模型的最大上下文窗口。Copilot CLI 会在需要时自动压缩对话；也可以使用斜杠命令查看或手动压缩上下文。

1. 返回 Codespace。如果 Copilot CLI 尚未打开，请从存储库根目录启动。
2. 输入：

   ```plaintext
   /context
   ```

3. 查看模型、当前 token 用量，以及系统指令、工具、消息和可用空间的上下文分布。
4. 压缩对话：

   ```plaintext
   /compact
   ```

5. 再次输入 `/context` 并比较结果。如果对话本来就很短，变化可能不明显。

> [!NOTE]
> Copilot CLI 会在上下文窗口接近上限时自动压缩上下文。使用 `/compact` 可以自行选择压缩时机。如果要切换到无关任务并开始全新对话，可以使用 `/clear` 或 `/new`。

## 选择模型

不同模型各有优势，开发者也有不同偏好。Copilot CLI 可以列出模型并选择要使用的模型。

1. 输入：

   ```plaintext
   /model
   ```

2. 查看可用模型和用量信息。
3. 保留当前模型、选择其他模型，或按 <kbd>Esc</kbd> 关闭列表。

## 共享会话

团队成员共享经验有助于彼此提升。`/share` 命令可以将会话导出为 Markdown 或 HTML 文件、创建可共享链接，或发布 GitHub gist。

1. 输入 `/help`，查看当前安装版本中的 `/share` 选项。
2. 如果要共享此会话，请选择合适的目标，例如 `/share file`，导出本地 Markdown 文件。
3. 发送或发布导出内容前先审查内容。会话导出可能包含提示词、回复和项目详情。

发布链接或 gist 是可选操作。不要发布团队无意共享的存储库或对话内容。

## 可选：扩展工作或委派任务

核心研讨会已经结束。Copilot CLI 还提供了适用于较大任务的命令：

- `/fleet` 可以将互不依赖的子任务分配给子代理并行执行。
- `/worktree` 可以为单独任务创建隔离的 Git 工作树。
- `/delegate` 可以将任务发送给 Copilot 云智能体；它会异步工作，并可能创建拉取请求。

这些命令是可选的，因为它们可能创建其他工作树或远程工作。尝试前先创建一个范围明确的新任务，并通过常规工作流审查结果。如需深入了解异步代理工作，可以继续学习[云智能体研讨会][cloud-workshop]。

## 总结和后续步骤

Copilot CLI 斜杠命令可用于配置 CLI、共享会话，以及查看其后台运行情况。本课中，你：

- 使用 `/context` 和 `/compact` 探索 Copilot 如何管理上下文。
- 使用 `/model` 查看可用模型。
- 了解了 `/share` 如何导出或共享会话。
- 探索了并行工作、工作树和委派给云智能体等可选命令。

Copilot CLI 还有更多斜杠命令等待探索！接下来，[回顾已学内容][next-lesson]，并了解后续学习方向。

## 资源

- [Copilot CLI 命令参考][cli-reference]
- [在 Copilot CLI 中管理上下文][context-management]
- [关于 Copilot 云智能体][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
