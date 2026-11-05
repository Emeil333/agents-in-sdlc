---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：代理模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要按照特定方式完成，或者 Copilot 需要了解一些背景信息，就要确保它可以获取这些上下文。可以通过多种方式向 Copilot 提供具体上下文，其中一种重要方式是[指令文件][instruction-files]，它可以为代码生成提供指导。

本练习将学习如何：

- 通过存储库自定义指令和按路径应用的指令文件，向 Copilot 提供项目上下文、编码指南和文档标准。
- 在现有指令下生成筛选功能的第一个数据切片（发行商辅助函数）。
- 向 `.github/copilot-instructions.md` 添加新的全局存储库标准。
- 使用相同提示词重新生成代码，观察新标准如何影响结果。
- 将指令更新和筛选功能切片提交到 `main`，供下一练习使用。

> [!CAUTION]
> 生成的代码可能不完全遵循设置的标准。Copilot 具有非确定性。本练习旨在观察更新指令后行为的变化趋势，而不是要求输出逐字一致。

## 指令文件

### 场景

Tailspin Toys 有一套开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加描述文件用途的注释块。

通过指令文件，可以确保 Copilot 了解这些实践要求。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，帮助它理解代码风格和要求。可以说明首选编码约定、库以及注释风格，也可以创建适用于整个存储库或特定文件类型的指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送给 Copilot，适合存放与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md`：为特定任务或文件类型提供指南，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件只用于 Copilot Chat 的代码生成，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围，但示例项目展示了常见方法：

- 在 `copilot-instructions.md` 中集中存放项目级指导，例如项目简介、结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，保持说明清楚，并提供正反示例。

指令文件没有唯一的编写方式，AI 的使用方式也不止一种。可以通过实践找到适合项目的方法。

> [!TIP]
> 使用 GitHub Copilot 的项目都应准备一组完善的指令文件。浏览项目文件时，可以看到其中包含[界面更新][ui-instructions]和 [Astro][astro-instructions] 等任务的指令。
>
> Copilot 也可以帮助生成指令文件。不同产品界面的操作方式有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相应课程会在适用时介绍具体操作。
>
> 如需模板或入门示例，可以查看 [awesome-copilot][awesome-copilot]，其中包含指令文件、自定义代理和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

阅读存储库附带的指令文件，包括核心 `copilot-instructions.md` 和各类 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中查看。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件中的项目简介和 **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指南。这些信息适用于与 Copilot 的各类交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的文件，其中包含 Astro、Drizzle 数据层、测试等方面的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段，它使用相对于存储库根目录的 glob 来指定适用文件。例如，匹配 `**/*.test.ts` 的 TypeScript 测试文件都会应用这些指令。
5. 阅读项目专用的单元测试指令。
6. 打开 `.github/instructions/drizzle.instructions.md` 并滚动到文件末尾。留意它指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成可复用的小文件，并提供示例供 Copilot 参考。链接路径相对于指令文件所在位置。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但目前没有要求在代码中添加文档。接下来会添加 TSDoc 注释和文件注释头的规则。

## 为更改创建分支

按照最佳实践，为更改创建一个分支。

1. 返回上一个练习所用的 Codespace。
2. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 Codespace，打开 Copilot Chat 面板并选择 **New Chat**，开始全新对话。模式和模型选择因练习而异；相应步骤会在需要时说明。

先在当前指令文件下发送提示词，以便观察自定义指令的效果。稍后会更新指令文件，并再次发送相同提示词以比较结果。

1. 关闭之前练习留下的编辑器标签页，让 Copilot 只获取需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 了解辅助函数应放在哪里。
3. 在 Chat 视图的代理下拉菜单中选择 **Agent**，让 Copilot 可以修改文件。

   ![Chat 视图中的代理选择器截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并更新代码，通常会同时修改辅助函数文件和测试。
6. 留意生成的辅助函数是带类型的函数，以 `db` 客户端为第一个参数，并返回带类型的发行商数组。这些约定来自适用于 `src/lib/*.ts` 的 `.github/instructions/drizzle.instructions.md`。
7. 留意生成代码**缺少** TSDoc 文档注释和文件级注释头。

> [!CAUTION]
> Copilot 具有概率性，因此它有时可能会在没有明确要求的情况下添加文档注释。这也没问题；本练习的重点仍是观察添加指令后行为的一致性变化。

## 添加新的存储库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在更新存储库编码标准，改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，约在第 27 行。留意其中记录了项目编码标准，但没有代码文档规则，因此刚才生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级别的 `.github/copilot-instructions.md` 中，提供全局指导；也可以放在 `*.instructions.md` 文件中，适用于特定语言、文件类型或任务。刚添加的文档注释规则属于项目级标准，因此适合放在存储库级文件中。

## 重新发送提示词并观察变化

1. 返回 Copilot Chat 并选择 **New Chat**，清空上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前相同的提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 留意文件开头现在有类似以下内容的注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 留意函数现在包含类似以下内容的 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

现在，你已引导 Copilot 遵循新的项目标准，并将其应用到下一练习会用到的真实代码中。

## 提交指令并推送分支

指令文件和其他资源一样，也需要使用源代码管理。现在将更改提交并推送到存储库。

1. 按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）打开终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为辅助函数基础所做的所有更改：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 将分支推送到存储库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支已推送，现在创建拉取请求，并关联待办事项中的文档标准议题。Copilot 可以搜索议题、创建用于关闭议题的 PR，然后合并 PR。

> [!NOTE]
> 这是第一个让 Copilot 代表你在 GitHub 上操作的练习。项目模板已配置 GitHub 工具，因此无需额外设置。Copilot 首次使用这些工具时，VS Code 可能会提示你登录 GitHub。按提示授权即可。后续练习会介绍这种连接如何工作（Model Context Protocol）。

1. 在 Codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认在代理选择器中选中 **Agent**。
3. 让 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新发行商辅助函数：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. 当 Copilot 提示允许其代表你执行 GitHub 操作时，检查操作内容，并根据需要选择 **Allow**。
6. 创建 PR 后，让 Copilot 合并 PR，并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 当 Copilot 提示允许其执行 GitHub 操作和 shell 命令时，检查命令并根据需要选择 **Allow**。

现在，你已使用 GitHub Copilot 创建并合并了拉取请求。

## 总结和后续步骤

你探索了 Copilot 如何读取项目指令，并在 VS Code 中使用 Copilot Chat：

- 发送代码生成提示词，观察现有指令的影响。
- 向 `.github/copilot-instructions.md` 添加全新的存储库标准。
- 再次发送相同提示词，观察生成代码如何遵循新标准。
- 将指令更新和辅助函数基础提交到 `main`，供下一练习使用。

接下来，让 Copilot 在[代理模式][next-lesson]下使用这些指令，跨代码库添加新功能。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot：指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
