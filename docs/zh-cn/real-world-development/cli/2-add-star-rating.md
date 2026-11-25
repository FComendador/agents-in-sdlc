---
title: "课程 2 - 添加星级评分：快速改进"
description: "使用 Copilot CLI 在游戏卡片上显示评分，通过转发的浏览器检查更改，并合并你的第一个拉取请求。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已安装 Copilot CLI 并尝试了对话。现在，你将首次更改项目。更改很小：游戏数据中已经有评分，但首页的游戏卡片尚未显示它。你将要求代理在每张卡片上显示现有评分，检查更改，然后将其作为你的第一个拉取请求合并。

在本课程中，你将：

- 从一个分支启动专注于某项功能的 Copilot 对话。
- 请求代理对项目进行一项小更改。
- 使用 `/diff` 检查更改。
- 运行应用，并在转发的浏览器中检查更改。
- 打开并合并你的第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，该评分已经显示在游戏详情页上。但是，首页卡片只显示标题、类别、发行商和描述。首先，你将要求代理在每张卡片上显示现有评分：这是一项范围明确的小更改，非常适合首次使用。

## 对话剖析

**对话**是你使用 Copilot CLI 处理任务的工作区。与 Copilot 应用不同，普通的 CLI 对话使用终端当前的 Git 仓库和分支，而不是创建专用工作树。你可以返回已保存的对话；文件和分支仍然是磁盘上常规的 Git 状态。

在对话中，你会看到三种内容：你的提示和代理的响应、代理浏览和编辑文件时的工具活动，以及可通过 `/diff` 检查的更改。

## 启动对话并请求更改

启动一个新对话来实现此功能。

1. 返回 Codespace。
2. 如果终端未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux)。
3. 如果 Copilot CLI 尚未运行，请执行：

   ```bash
   copilot --yolo
   ```

4. 输入斜杠命令 `/new`，然后按 <kbd>Enter</kbd> 启动新会话。
5. 使用此提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 将浏览项目，找到显示游戏详情的文件，并创建所需代码。你已使用 Copilot CLI 添加了一项功能。

## 检查 diff

合并之前，必须检查所有 AI 生成的更改，即使是小改动也不例外。在 Copilot CLI 中检查更改。

1. 输入 `/diff`，检查所有已更改的文件。
2. 确认卡片在有评分时显示数值，并在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种状态。
4. 检查 Copilot 运行的检查结果，并要求它修复任何失败项。
5. 检查完成后，按 <kbd>Esc</kbd> 退出 diff 视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的行为是概率性的，而非确定性的，因此生成的具体代码可能会不同。请检查行为，而不要期待每次都得到完全相同的实现。

## 验证更改

仅阅读代码并假定其正常工作是不够的。请 Copilot 启动网站，以便在 Codespaces 转发的浏览器中检查更新后的用户界面 (UI)。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. 当 Codespaces 指示端口 `4321` 可用时，选择 **Open in Browser**。
3. 确认游戏卡片显示五星制评分。
4. 返回 Copilot，并要求它停止已启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 打开并合并你的第一个拉取请求

你已构建此功能。现在，你将创建一个拉取请求 (PR)，将新代码合并到项目中。

1. 请默认代理提交此更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 会在创建 PR 时推送现有提交，并显示 URL。
3. 按住 <kbd>Command</kbd> (Mac) 或 <kbd>Ctrl</kbd> (Windows/Linux)，然后选择 Copilot CLI 显示的 URL 以打开 PR。
4. 检查已更改的文件和检查结果。
5. 一切准备就绪后，选择 **Merge pull request** 并确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main`：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！你已使用 GitHub Copilot CLI 发布了第一个更改。本课程中，你：

- 从一个分支启动了专注于某项功能的 Copilot 对话。
- 请求代理对游戏卡片进行了一项小更改。
- 使用 `/diff` 检查了更改。
- 运行了应用，并在转发的浏览器中检查了评分。
- 打开并合并了你的第一个拉取请求。

接下来，[从筛选 issue 入手，使用 Plan 和 Autopilot 模式][next-lesson]开发更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
