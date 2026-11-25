---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前两项练习中，你让 GitHub Copilot 云智能体执行了三项任务，以改善用户体验并添加功能。云智能体会异步、自主地工作，但仍需监控任务。

有多种工具可用于管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个控制中心，可以查看所有具有未关闭拉取请求的智能体任务、检查已执行的操作，还可以指导正在进行的会话。

本课将介绍如何：

- 浏览智能体页面，监控云智能体任务。
- 指导正在进行的会话，要求添加其他功能。

## 场景

将创建高对比度模式的任务分配给智能体后，团队意识到也可以趁此机会添加浅色模式。既然正在更新网站样式并添加模式切换功能，将浅色模式一并纳入就很合适。你将指导智能体，同时添加浅色模式和高对比度模式。

## 查看 GitHub Copilot 云智能体任务

查看分配给云智能体的任务当前状态。

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。应能看到分配给 Copilot 的任务，包括：
   - 更新代码库文档。
   - 生成用于修改游戏的 API。
   - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 已执行的操作。可能包括：
   - 从存储库检出代码。
   - 为 Copilot 创建工作环境。
   - 设置 MCP 服务器。
   - 执行多个步骤来完成分配的任务。

> [!NOTE]
> 显示的具体步骤会根据 Copilot 的工作状态和采取的方法而有所不同。

4. 还要查看右侧显示的拉取请求面板。可以在其中查看拉取请求和已更改的文件，以便进一步监控。

## 指导云智能体

了解当前任务后，要求 Copilot 在实现高对比度模式的同时也添加浅色模式。

1. 选择与添加高对比度模式相关的会话。确切标题取决于 Copilot 使用的名称和当前工作状态。

    ![控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到它显示设置已完成并开始工作。出现类似以下消息时，即表示已开始。
3. 在 **Steer active session while Copilot is working** 对话框中添加以下提示词：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务，突出显示了指导活动会话的对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示词。
5. 查看 Copilot 如何确认提示词并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务。它会先完成收到消息时正在执行的步骤，再将新请求纳入工作流程。

这也需要几分钟。可以利用这段时间回顾迄今为止学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，它是监控和指导 GitHub Copilot 云智能体任务的中心位置。在此控制中心中，可以跟踪所有正在进行和已完成的任务、审查工作内容，并调整执行中的任务范围或提供更多指导。

本课介绍了以下内容：

- 浏览控制中心和智能体页面，监控云智能体任务。
- 重定向正在进行的会话，要求添加其他功能。

Copilot 正在完成无障碍功能。接下来，[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
