---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "探索存储库指令，添加文档标准，并将其应用到筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，应让 Copilot 能够获取相应指导。[指令文件][instruction-files]不仅说明希望编写什么代码，也说明代码应如何组织。完成筛选功能后，现在可以了解 Copilot 使用的指令、添加文档标准，并将其应用到代码中。

本课将学习如何：

- 了解存储库指令和按路径应用的指令文件如何提供给代理。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 注释应解释意图和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果函数接受可注入的 `db` 参数，也要记录该参数。
- 可复用 Astro 组件应说明其 `Props` 接口；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指南。

通过指令文件，可以确保 Copilot 了解这些实践要求。

## 指令文件

自定义指令可向 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。可以说明编码约定、首选库以及注释风格，也可以创建适用于整个存储库或特定文件类型的指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot 的指令文件。
- `.github/instructions/*.instructions.md`：针对特定语言、文件类型或任务提供指南。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。使用特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

项目已包含一组指令文件。先了解现有内容，再进行更改，以便观察更改的效果。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而不是终端）中打开 `.github/copilot-instructions.md`。
3. 查看文件中的项目简介和编码指南。这些指令适用于该存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件，其中包含 Astro、Drizzle 数据层、测试等方面的指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它使用 glob 指定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目现有文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并提供示例供 Copilot 参考。

## 更新指令以符合团队指南

现有文件已经提供了不错的基础。现在修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 遵循 TSDoc 标准。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，约在第 35 行。
2. 在该部分末尾添加以下项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存更改后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指导。

1. 如果筛选对话仍处于打开状态，输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支上恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 让 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看更改的 TypeScript 文件。留意新生成的 TSDoc 注释，并确认其准确描述代码。

## 总结和后续步骤

你已了解 Copilot 如何读取项目指令，并新增标准以指导功能代码。具体来说，你：

- 了解了存储库指令和按路径应用的指令文件如何提供给代理。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
