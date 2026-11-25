---
title: "课程 4 - 使用自定义说明引导 Copilot"
description: "探索仓库说明，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果某项任务必须以特定方式完成，请确保 Copilot 可以获取相关说明。[说明文件][instruction-files]不仅描述你希望代码*做什么*，还描述它应该*如何*组织。现在你已构建筛选功能，接下来将检查 Copilot 使用的说明、添加一项文档标准，并将其应用于代码。

在本课程中，你将：

- 探索仓库说明和适用于特定路径的文件如何传递给代理。
- 更新说明文件，确保遵循编码标准。
- 查看说明文件对代码的影响。

## 场景

Tailspin Toys 对开发实践有以下准则和要求：

- 注释应解释意图和不明显的决策，而不是重复代码。
- `db/` 和 `src/lib/` 中导出的函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果有可注入的 `db` 参数，也要说明。
- 可复用的 Astro 组件应记录其 `Props` 契约；相关代码更改时应同步更新注释。
- 保留现有格式设置和 lint 说明。

通过说明文件，你将为 Copilot 提供遵循这些实践所需的信息。

## 说明文件

自定义说明会向 Copilot 提供上下文和偏好，帮助它了解你的风格和要求。你可以指定约定、库和注释类型。说明可以适用于整个仓库，也可以针对特定文件类型和任务。

说明文件有两种：

- `.github/copilot-instructions.md` 是一个单独文件，会在仓库中的**所有**请求中发送给 Copilot。它应包含与大多数请求相关的项目级信息。
- `.github/instructions/*.instructions.md` 包含适用于特定语言、文件类型或任务的指南。

> [!NOTE]
> 说明格式及其支持情况因环境而异。依赖特定格式之前，请查看[自定义说明支持参考][custom-instructions-support]。

## 探索项目的自定义说明文件

入门项目已包含多个说明文件。进行更改之前，先探索这些文件并了解其作用。

1. 返回 Codespace。
2. 在 Codespaces 编辑器中（而不是终端中）打开 `.github/copilot-instructions.md`。
3. 查看简短的项目描述和编码指南。它们适用于此仓库中与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并浏览其中的文件。这里有针对 Astro、Drizzle 数据层、测试等内容的说明。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段：它定义了用于确定说明适用文件的 glob。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他说明文件和现有项目文件的链接。这样可以将较大的说明集拆分成较小、可复用的文件，并指出 Copilot 应遵循的示例。

## 根据团队指南更新说明文件

现有文件是不错的起点，但还缺少一条规则。修改主 `copilot-instructions.md` 文件，要求新的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，该部分应位于第 35 行附近。
2. 在列表末尾添加此项目：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 应用更新后的说明

Copilot CLI 会在启动对话时加载仓库说明。保存说明后，退出并恢复筛选对话，让 Copilot 在保留功能上下文的同时加载新指南。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 将其关闭。Copilot CLI 会自动保存对话。
2. 在仓库根目录的 `game-filters-cli` 分支上，恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果系统提示，请选择筛选对话，并确认选中的是 `game-filters-cli` 分支。
3. 要求 Copilot 应用更新后的指南：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并检查已更改的 TypeScript 文件。查看新添加的 TSDoc 注释，并确认它们准确描述代码。

## 总结和后续步骤

你已探索 Copilot CLI 如何从说明文件获取上下文，并将新标准应用于功能。本课程中，你：

- 探索了仓库说明和适用于特定路径的文件如何传递给代理。
- 更新了说明文件，确保遵循编码标准。
- 查看了说明文件对代码的影响。

接下来，[自定义并运行可复用的 `quality-checks` 技能][next-lesson]，以执行相同的 lint 和测试检查。

## 资源

- [向 Copilot CLI 添加自定义说明][instruction-files]
- [自定义说明支持][custom-instructions-support]
- [编写有效自定义说明的最佳实践][instructions-best-practices]
- [Awesome Copilot：说明文件和其他资源集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
