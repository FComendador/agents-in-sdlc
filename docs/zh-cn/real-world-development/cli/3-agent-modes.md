---
title: "课程 3 - 代理模式：Plan 和 Autopilot"
description: "使用 Plan 确定方法，使用 Autopilot 根据 issue 实现筛选功能，并使用 Interactive 检查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们从向项目添加一项小功能开始。更大的更改需要更完善的流程。GitHub Copilot CLI 可以集成到组织的工作流中，帮助我们以正确的方式开发正确的内容。在本课程和后续课程中，你将遵循常见的代理式开发流程：从 issue 开始，构建功能，检查代码是否有效且行为符合预期，最后将其合并到项目中。

> [!NOTE]
> 你将使用相同的对话和分支继续此功能工作流。通常，不同类型的文件会使用不同分支或 PR，但我们会简化其中一些步骤，以便专注于核心概念。

在本课程中，你将：

- 从 GitHub issue 启动新的 Copilot 对话。
- 在 Plan 模式下确定需求。
- 使用 Autopilot 模式实现功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

在后续课程中，你将更新仓库说明，自定义现有的 `quality-checks` 技能，添加 MCP 验证，创建 QA 代理，并打开功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的 issue 描述了这项功能，但在编写代码之前，需要商定类别如何组合等细节。你将使用 Plan 做出这些决定，然后使用 Autopilot 执行范围明确的实现。

## 背景

将 AI 编码代理添加到开发工作流并不会改变基本原则，反而会让这些原则更加重要。大多数开发者遵循类似的流程：

1. 从描述工作的已记录 issue 开始。
2. 制定要构建内容的计划。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码审查完成且持续集成 (CI) 检查通过后，合并更改。

> [!NOTE]
> 具体细节因团队和组织而异，但大多数流程都是这一方法的变体。

这种标准流程有助于确保 AI 生成的代码满足需求，并接受与手写代码相同的审查。

## 对话模式

**对话模式**控制代理的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在各模式间切换：

- **Interactive**：你与代理一起工作。代理会提出更改建议，并在继续之前等待你的回应。
- **Plan**：代理先创建计划，且不能编辑项目文件。
- **Autopilot**：代理自主工作，编写代码、运行测试并反复迭代，直至完成任务。

先在 Plan 模式下制定并检查计划，然后使用 Autopilot 实现计划。

## 从 issue 开始

处理筛选功能之前，请返回 Codespace，并确认仓库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请重新启动。
2. 确认评分 PR 已合并。
3. 如果终端未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux)。
4. 更新 `main` 并为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有 issue。
8. 使用箭头键突出显示 **Allow users to filter games by category and publisher**。
9. 按 <kbd>C</kbd> 将 issue 添加到提示中，并返回 **Session** 选项卡。

现在，提示会以 `#7` 或类似编号开头。`#` 可将 GitHub issue 或 PR 添加到上下文中。

## 规划筛选功能

规划可以帮助你在将功能实现或任务委托给 Copilot 之前确定方法。复杂任务最好花时间规划。切换到 Plan 模式，并要求 Copilot 创建计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的指示器显示 **Plan**。
2. 在上一步添加的 issue 引用后输入：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 将探索项目，并确定制定计划的最佳方法。

3. Copilot 可能会询问筛选功能应如何工作。根据偏好回答即可；没有错误答案。
4. 计划准备好后，按 <kbd>Control</kbd>+<kbd>E</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) 展开计划。
5. 滚动浏览并检查计划。
6. 如果计划中的任何内容与你的决定不符，请要求 Copilot 修改。

## 批准 Autopilot

你已编写并审查计划。现在，使用 Autopilot 实现计划。

Autopilot 允许 Copilot 反复迭代，直到它认为任务已经完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或你所用版本中的相应选项。
2. 确认提示下方的指示器显示 **Autopilot**。
3. 观察 Copilot 遵循计划、生成代码并运行测试。

> [!NOTE]
> 批准计划后，实现可能立即开始；请先检查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决配置问题，再认为检查已完成。

## 审查并验证实现

与其他代码一样，生成的代码在合并前也必须经过审查。你将检查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 将结果与 issue 和规划时做出的决定进行比较。
4. 检查代码后，按 <kbd>Esc</kbd> 退出 diff 视图。
5. 检查项目验证结果，并要求 Copilot 修复任何失败项。

## 试用新功能

代码看起来正确，但它能否运行？像之前一样启动应用，并通过 Codespaces 转发的端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 指示端口 `4321` 可用时，选择 **Open in Browser**。
3. 根据计划测试类别筛选、发行商筛选以及筛选组合。
4. 确认重置筛选和无结果状态符合 issue 和你的决定。
5. 返回 Codespace，并要求 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

你使用不同的对话模式构建并检查了一项功能。本课程中，你：

- 从 GitHub issue 启动了新的 Copilot 对话。
- 在 Plan 模式下确定了需求。
- 使用 Autopilot 模式实现了功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，[使用自定义说明][next-lesson]了解如何按照记录的实践生成代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
