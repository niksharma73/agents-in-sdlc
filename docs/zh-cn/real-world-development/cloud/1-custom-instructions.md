---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取这些上下文。[指令文件][instruction-files]可提供相关指导，帮助 Copilot 不仅了解要做什么，也了解应如何完成。

本练习将帮助你：

- 了解如何通过存储库自定义指令和按路径应用的指令文件，为 Copilot 提供项目上下文、编码规范和文档标准。
- 在 `.github/copilot-instructions.md` 中添加一项适用于整个存储库的新标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会比较提示的前后效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体生成的拉取请求时，你会看到指令文件的作用。

## 指令文件

### 场景

Tailspin Toys 和其他优秀的开发团队一样，制定了一系列开发规范和要求，例如：

- 数据层始终需要单元测试。
- UI 应采用暗色模式并具有现代感。
- 应使用 TSDoc 文档注释为代码添加说明。
- 每个文件开头都应添加一段注释，说明文件的用途。

通过使用指令文件，你可以确保 Copilot 了解这些规范，并据此完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它更好地理解你的编码风格和要求。你可以通过这种强大的功能引导 Copilot 提供更相关的建议和代码片段。例如，你可以指定偏好的编码约定、库，以及注释风格。你可以为整个存储库创建指令，也可以针对特定文件类型或任务创建指令。

指令文件有两种：

- `.github/copilot-instructions.md`：每次针对该存储库向 Copilot 发出请求时，都会发送这个指令文件。它应包含项目级信息，即大多数面向 Copilot 的聊天或 CLI 请求都适用的上下文，例如技术栈、项目概述、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可针对特定任务或文件类型创建这些文件。例如，可为 TypeScript 或 Astro 等语言，或创建 UI 组件、单元测试等任务提供指导。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时，都会使用存储库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

在此深入讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了具有代表性的方法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、项目结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（例如单元测试、Astro 组件和数据层）或特定任务提供指导。
- 使用自然语言，并让指导清晰易懂。提供代码示例，说明什么样的代码符合或不符合要求。

创建指令文件没有唯一的方法，使用 AI 也是如此。通过试验，你会逐渐找到适合项目和场景的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应包含完善的指令文件。浏览本项目中的指令文件时，你会看到它们涵盖了多种任务，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助你生成指令文件。不同产品界面的操作方式各不相同（例如，VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`）。相应产品的课程会在适用时介绍具体操作。
>
> 想查找模板或起点？可以浏览 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 浏览本项目中的自定义指令文件

花些时间阅读此存储库自带的指令文件：其中有一个核心文件 `copilot-instructions.md`，还有一组适用于各种任务的 `*.instructions.md` 文件。你可以在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。使用 Copilot 时，这些内容都可能适用。
3. 打开 `.github/instructions` 文件夹并查看其中的文件。这里有适用于 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。留意顶部的 `applyTo` 字段；它设置相对于存储库根目录的 glob，用于指定指令适用的文件。此处与 TypeScript 测试文件匹配，例如符合 `**/*.test.ts` 的文件。
5. 留意专门用于在本项目中创建单元测试的指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。留意指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样就能将较大的指令集拆分成更小、可重复使用的文件，并向 Copilot 提供可参考的示例。这些路径相对于指令文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目的编码标准，但目前尚未要求在代码中添加文档注释。接下来，你将添加 TSDoc 文档注释和文件注释头的规则。

## 添加存储库级标准

接下来要做的唯一编辑，是添加一条适用于整个项目的规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后在本环境中分配议题时，云智能体会遵循这条规则。

编辑前，先创建并切换到一个分支（云智能体会读取议题目标分支中的指令文件，但你仍应规范地提交这些更改）：

1. 在 codespace 终端中，创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加编码标准，要求在代码中添加文档说明，从而改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分（应位于第 27 行附近）。留意该部分如何介绍项目的编码标准，但目前并未要求在代码中添加文档说明，因此生成的辅助函数没有文档注释。
3. 在现有标准下方添加以下 Markdown 内容，以要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以在存储库级别（`.github/copilot-instructions.md`）创建全局指令文件，也可以针对特定语言、文件类型或任务创建 `*.instructions.md` 文件。存储库级文件适合放置适用于整个项目的标准，例如刚添加的文档注释规则。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令文件。在下一课中将议题分配给 Copilot 时，它会从 `main` 分支创建分支。因此，必须先将指令更新合并到 `main`，云智能体才能使用。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上，创建一个从 `custom-instructions-cloud` 合并到 `main` 的拉取请求并将其合并。下一课中处理议题时，云智能体就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main` 分支，可以跳过创建分支的步骤，并直接提交到 `main`。此处创建分支是为了让研讨会更贴近实际项目中的工作方式。

## 总结与后续步骤

你了解了 Copilot 如何从项目的指令文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了一项适用于整个存储库的新标准。接下来审查云智能体生成的拉取请求时，你会看到它如何遵循这项标准。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
