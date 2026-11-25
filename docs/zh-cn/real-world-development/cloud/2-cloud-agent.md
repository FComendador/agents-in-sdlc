---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会遇到技术债务，例如尚未解决的安全问题、需要更新的旧代码，或因缺少时间而长期留在待办事项中的功能请求。GitHub Copilot 云智能体可以自主执行更新代码、添加功能等任务。完成后，它会创建草稿拉取请求，供开发人员审查。这样可以委派繁琐任务、加快开发进度，并让开发人员专注于更重要的事项。

本练习将介绍如何使用云智能体：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 了解明确界定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些需要解决的技术债务。最初受聘开发网站的承包商没有妥善编写文档——实际上，你会发现文档完全缺失。团队希望首先为应用程序中所有导出的函数添加 TSDoc 文档注释。

此外，设计团队也准备改进游戏发现体验。他们希望在每款游戏的详情页上显示相关游戏（同一类别中的其他作品），让用户可以继续浏览。目前不要求实现得很精致，只要能用于用户体验 (UX) 验收测试即可。这项工作目前是阻塞项，不过还有优先级更高的议题。

这类任务很容易被降低优先级，因此适合分配给 GitHub Copilot 云智能体。云智能体可以异步处理任务，让开发人员先处理其他工作，之后再回来审查结果并确认是否符合预期。

## 认识 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像开发人员一样在后台执行任务。你也可以像分配给开发人员一样，[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。分配后，Copilot 会创建草稿拉取请求以跟踪进度、设置工作环境并开始处理任务。你可以在会话进行中或完成后查看详情。建议的解决方案准备好供审查时，Copilot 会在拉取请求中提及你。

## 明确界定指令范围的重要性

GitHub Copilot 并非魔法，思考工作仍不可少。看似简单的操作也可能包含复杂之处，因此请[谨慎界定分配给 GitHub Copilot 云智能体的任务范围][cloud-agent-best-practices]。把它当作 AI 结对程序员：分阶段工作、不断学习和试验，并根据结果调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 本存储库中的自定义指令

前面的练习介绍了自定义指令及其如何指导 Copilot。如果已经完成自定义指令的实践练习，就看过 `.github/copilot-instructions.md` 的作用；如果还没有，现在可以快速查看。分配任务给云智能体之前，请只读检查本存储库中已有的指令文件，稍后再观察它们的影响。

在存储库的 GitHub Web 界面或正在运行的 codespace 中打开以下文件：

- `.github/copilot-instructions.md`：查看 **Code standards** 部分，尤其是 TypeScript 规范和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，了解这些指令如何应用于 `**/*.test.ts` 文件。

在下一部分将 `Code lacks documentation` 议题分配给云智能体时，检查生成的拉取请求中是否包含 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自指令文件。后续练习审查拉取请求时会再次检查。

## 设置 GitHub Copilot 云智能体的开发环境

无论由谁编写代码，通常都需要特定环境并运行一些设置脚本，确保初始状态正常。将任务分配给以类似软件工程师方式工作的 Copilot 时也是如此。

云智能体使用 [GitHub Actions][github-actions]作为工作环境。可以在 `.github/workflows/copilot-setup-steps.yml` 中配置[专用设置工作流][setup-workflow]，让它在开始工作前运行，以便获得所需的开发工具和依赖项。本研讨会已预先配置此工作流，以便顺利完成练习。它会为单一 Astro 应用提供 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及已迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

它看起来与其他 GitHub 工作流文件相似，但有几个要点：

- 它包含一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求前，GitHub Actions 会先运行此作业。
- 它使用 `workflow_dispatch` 触发器，因此可以从存储库的 **Actions** 选项卡手动运行工作流。这样无需等待 Copilot 启动工作流，也能检查它是否能正常运行。

## 添加文档

虽然大家都知道文档很重要，但许多项目的信息已经过时，或者完全没有文档。这类技术债务经常得不到处理，降低效率，也让维护代码库或引入新开发人员变得更加困难。Copilot 擅长创建文档，因此可以把这项工作分配给云智能体。它会在后台生成所需文档，稍后再回来审查结果。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**。
5. 在 **Title** 中输入 `Code lacks documentation`。
6. 在 **Description** 中输入：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

    ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开刚创建的拉取请求 (PR)，标题可能类似于 `[WIP]: Code lacks documentation`。如果列表中尚未出现，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，Copilot 应该会创建待办事项列表。

> [!NOTE]
> Copilot 的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建工作环境（运行前面介绍的工作流）、分析项目并确定解决问题的最佳方法。

13. 查看列表以及 Copilot 将要完成的任务。
14. 向下滚动拉取请求时间线，查看 Copilot 已开始处理议题的更新。
15. 选择 **View session** 按钮。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的状态指示器。

16. 浏览实时会话，观察 Copilot 如何解决问题，包括探索代码并了解当前状态、暂停思考并确定计划，以及编写代码。

这可能需要几分钟。云智能体的一项主要优势是能够异步执行任务，让你腾出时间处理其他工作。现在就利用这一点：再分配一个任务给云智能体，同时转而为应用程序添加功能。

## 在游戏详情页中添加相关游戏部分

GitHub Copilot 云智能体的一项优势是可以拆分工作：你专注于一组任务，它则处理另一组任务。即使为设计团队添加相关游戏部分不会花很久，这段时间也可以用于其他工作。现在将这项任务也分配给云智能体。

1. 返回 github.com 上的存储库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue**，打开新议题对话框。
4. 选择 **Blank issue**。
5. 在 **Title** 中输入 `Show related games on the game details page`。
6. 在 **Description** 中输入：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

稍后，议题的第一条评论中会出现 👀，表示 Copilot 正在处理任务。

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 正在处理第二项请求。云智能体的工作方式类似软件工程师，因此无需一直监控，完成后再审查即可。接下来，了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。可以将议题分配给云智能体，让它异步处理技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

本课介绍了以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确界定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体在后台工作时，可以开始创建和使用自定义智能体。[GitHub Copilot 云智能体也可以使用 MCP 服务器][cloud-agent-mcp]，并可使用前面模块介绍的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流的最佳做法][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
