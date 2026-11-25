---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：GitHub Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能访问这些上下文。[指令文件][instruction-files]可提供相关指导，帮助 Copilot 理解你希望它做什么以及如何完成。

本练习将介绍如何：

- 通过存储库自定义指令和适用于特定路径的指令文件，将项目上下文、编码指南和文档标准传递给 Copilot。
- 在 `.github/copilot-instructions.md` 中添加一项适用于整个存储库的新标准。

> [!NOTE]
> 与 VS Code 和 CLI 环境不同，本练习不会通过前后提示词来对比结果。GitHub Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，可以看到指令文件发挥的作用。

## 指令文件

### 场景

与其他优秀的软件开发团队一样，Tailspin Toys 也有一套开发实践指南和要求：

- 数据层必须始终包含单元测试。
- UI 应使用暗色模式并具有现代风格。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头都应有注释块，说明该文件的用途。

使用指令文件为 Copilot 提供必要信息，使其按照这些实践完成任务。

### 自定义指令

自定义指令可向 Copilot 提供上下文和偏好设置，帮助它理解你的编码风格和要求。你可以指定编码规范、库，甚至希望代码包含的注释类型。可以创建适用于整个存储库的指令，也可以针对特定文件类型或任务创建指令。

指令文件有两种类型：

- `.github/copilot-instructions.md` 是一个指令文件，会随存储库中的**每个**请求发送给 Copilot。文件应包含对大多数聊天或 CLI 请求都适用的项目级信息，例如技术栈、项目概述、最佳做法和全局指导。
- `.github/instructions/*.instructions.md` 文件适用于特定任务或文件类型。你可以为 TypeScript 或 Astro 等语言制定指南，也可以为创建 UI 组件或单元测试等任务制定指南。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 GitHub Copilot 云智能体生成代码时，会使用存储库级指令以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还[支持其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳做法

详细讲解如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。总体而言：

- 让 `copilot-instructions.md` 专注于项目级指导，例如项目说明、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（单元测试、Astro 组件、数据层）或特定任务提供指导。
- 使用自然语言，清楚地编写指南，并提供代码应当和不应当如何编写的示例。

创建指令文件没有唯一的正确方式，使用 AI 也是如此。通过试验找到最适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应准备一套完善的指令文件。浏览本项目中的文件时，可以看到针对许多任务的指导，包括 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> 也可以让 Copilot 帮助生成指令文件。不同环境的入口有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前环境的课程会在相关位置介绍具体方法。
>
> 如需查找模板或起点，请浏览 [awesome-copilot][awesome-copilot] 存储库，其中包含指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看本项目中的自定义指令文件

阅读此存储库附带的指令文件：一个核心 `copilot-instructions.md` 文件，以及一组适用于不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。留意 **Code standards** 下的 **GitHub Actions Workflows** 指南。这些内容适用于与 Copilot 的所有交互。
3. 打开 `.github/instructions` 文件夹并浏览其中的内容。这里有针对 Astro 文件、Drizzle 数据层、测试等内容的指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。查看顶部的 `applyTo` 字段；它指定相对于存储库根目录的 glob，用于确定指令适用的文件。这里会匹配 `**/*.test.ts` 等 TypeScript 测试文件。
5. 阅读本项目针对单元测试的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。查看它指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分成较小、可复用的文件，并为 Copilot 提供生成代码时可参考的示例。这里的路径相对于指令文件，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分介绍了项目编码标准，但尚未要求在代码中添加文档。接下来将添加 TSDoc 文档注释和文件注释头的规则。

## 添加存储库级标准

接下来要进行本练习中唯一的一次编辑：添加一项适用于整个项目的规则，要求在代码中使用 TSDoc 文档注释和文件级注释头。稍后在本环境中向云智能体分配议题时，它会遵循这项规则。

编辑前先创建工作分支。云智能体会从议题的目标分支读取指令，同时你也会将更改整洁地提交。

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。添加存储库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到大约位于第 27 行的 **Code formatting requirements** 部分。查看现有编码标准，并注意其中尚未包含代码文档规则。
3. 在现有标准下方添加以下 Markdown 行，要求 Copilot 添加文件注释头和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所示，可以使用存储库级文件 (`.github/copilot-instructions.md`) 提供全局指导，也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指令。刚添加的文档注释规则属于项目级标准，应放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令文件。在下一练习中将议题分配给 Copilot 时，它会从 `main` 创建分支。因此，必须先将更改合并到 `main`，云智能体才能使用这些指令。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 指向 `main` 的拉取请求并将其合并。下一练习分配议题后，云智能体就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支操作并直接提交到 `main`。此处保留分支步骤，是为了模拟真实项目中的工作方式。

## 总结和后续步骤

你已了解 Copilot 如何从本项目的指令文件中获取上下文，并在 `.github/copilot-instructions.md` 中添加了存储库级标准。接下来可以在此环境中查看云智能体创建的拉取请求如何应用该标准。

下一步，[将第一个议题分配给 GitHub Copilot 云智能体][next-lesson]。

## 资源

- [用于自定义 GitHub Copilot 的指令文件][instruction-files]
- [创建自定义指令的最佳做法][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件及其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
