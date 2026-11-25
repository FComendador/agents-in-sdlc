---
title: "课程 8 - 创建并合并功能 PR"
description: "一起检查筛选功能、说明、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选实现、说明更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一个分支中。现在该一起检查这些内容并打开拉取请求了。星级评分拉取请求 (PR) 是你手动合并的；这一次，你将让 **Agent Merge** 管理流程。

> [!NOTE]
> 通常，我们会将功能、说明更新、技能更新和 QA 代理拆分成几个独立的 PR。为了简化研讨会，你将所有筛选和质量工作保留在同一个对话和分支中，并将其纳入此 PR。

在本课程中，你将：

- 了解 Agent Merge 是什么，以及它如何自动化合并生命周期。
- 检查完整的功能更改和验证证据。
- 创建筛选 PR。
- 仅在审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选工作流中，你使用 Copilot 规划、实现并验证了一项功能。现在，Tailspin Toys 希望自动处理 PR 的剩余工作，同时由开发者控制合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动处理合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的问题——修复失败的持续集成 (CI) 检查、回复审查意见，并在需要时进行变基——然后在仓库允许的情况下启用 GitHub 自动合并。

到目前为止，你都是自行选择 **Merge pull request**。Agent Merge 可以承担这一职责。启用之前，请先检查工作并确认已准备好合并。

## 使用 Agent Merge 管理 PR

代码都已完成，现在一起检查代码、创建 PR，并让 Agent Merge 管理剩余流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开代理对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建新的 PR。
5. 使用 `/pr agentmerge` 激活 Agent Merge。
6. Copilot 将监视 PR 的持续集成流程。一切成功后，它会执行合并。
7. 确认 Copilot 显示了类似 `"PR #14 was squash-merged successfully."` 的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、仓库设置或缺失的权限。如果操作被阻止，请阅读报告的原因，并在仓库允许时手动完成已审查的合并。

## 总结和后续步骤

你已自动化开发流程中的多个部分，包括代码生成、代码测试和验证，以及现在的拉取请求流程。本课程中，你：

- 了解了 Agent Merge 是什么，以及它如何自动化合并生命周期。
- 检查了完整的功能更改和验证证据。
- 创建了筛选 PR。
- 在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云代理委托。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
