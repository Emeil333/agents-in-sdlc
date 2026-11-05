---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它可以获取这些上下文。[指令文件][instruction-files]可以提供指导，让 Copilot 不仅了解要做什么，也了解如何完成任务。

本练习将学习如何：

- 通过存储库自定义指令和按路径应用的指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会比较更新指令前后的提示词。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令效果。后续审查云智能体创建的拉取请求时，可以看到指令文件的影响。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加描述文件用途的注释块。

通过指令文件，可以确保 Copilot 了解这些实践要求。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。可以说明编码约定、首选库以及注释风格，也可以创建适用于整个存储库或特定文件类型的指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot，适合存放与大多数请求相关的项目级信息，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指南，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

项目示例展示了常见方法：

- 在 `copilot-instructions.md` 中集中存放项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持说明清楚，并提供正反示例。

指令文件和 AI 的使用方式都没有唯一正确答案。通过实践找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应准备一组完善的指令文件。项目中的指令涉及多种任务，包括[界面更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应课程会在适用时介绍具体操作。
>
> 如需模板或入门示例，可以查看 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

花些时间查看存储库附带的指令文件，包括核心 `copilot-instructions.md` 和各类 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览项目简介以及 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指南。这些信息适用于与 Copilot 的各类交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件，其中包含 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 来指定适用文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 阅读项目专用的单元测试指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。留意它指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并提供示例供 Copilot 参考。链接路径相对于指令文件所在位置。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前没有要求在代码中添加文档。接下来会添加 TSDoc 注释和文件注释头的规则。

## 添加新的存储库标准

接下来要编辑一次项目：添加全局规则，要求在代码中使用 TSDoc 文档注释，并为每个文件添加文件级注释头。云智能体在本学习路径稍后的工作中会读取这些规则。

编辑前先创建工作分支。云智能体会读取议题目标分支中的指令，但仍建议单独提交这些更改：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在更新存储库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。留意其中记录了项目编码标准，但没有代码文档规则。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级别的 `.github/copilot-instructions.md` 中，提供全局指导；也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。新增的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一练习中将议题分配给 Copilot 时，它会从 `main` 分支创建新分支，因此必须先将指令更新合并到 `main`，云智能体才能读取。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上从 `custom-instructions-cloud` 向 `main` 创建拉取请求并合并。下一练习中处理分配给云智能体的议题时，它就能读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤并直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你探索了 Copilot 如何读取项目指令，并向 `.github/copilot-instructions.md` 添加了新的存储库级标准。后续云智能体创建的拉取请求会体现该标准。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
