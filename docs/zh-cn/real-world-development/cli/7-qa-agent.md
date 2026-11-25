---
title: "课程 7 - 创建并使用 QA 代理"
description: "创建一个 QA 自定义代理，将 issue 要求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动检查，并通过 Playwright MCP 在浏览器中观察筛选体验。现在，你将把这些功能整合到一个具有明确定义的 QA 流程的自定义代理中。

在本课程中，你将：

- 探索自定义代理如何结合说明、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 代理，并根据筛选 issue 检查其发现。

## 场景

Tailspin Toys 希望在打开拉取请求 (PR) 之前，始终一致地检查要求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义代理可以协调 QA 流程，并提供可复用的报告。

## 什么是自定义代理？

自定义代理是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件描述代理的用途、说明和可用工具。在本研讨会中，你将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

你使用的自定义功能各有不同用途。仓库说明描述团队标准。`quality-checks` 技能封装可重复执行的检查。Playwright MCP 提供浏览器工具。QA 配置文件告诉 Copilot 如何使用这些能力评估要求并报告发现。它不会取代其他功能，也不需要新对话。

## 创建 QA 配置文件

打开功能 PR 之前，你将请 Copilot 创建一个可复用的 QA 配置文件。该配置文件会定义 QA 执行的检查和必须遵守的边界。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请默认代理创建新的自定义代理：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新代理之前，请检查其配置文件，确认 Copilot 捕获了预期的 QA 工作流和边界。这样可以避免不完整或范围过大的代理在你只想验证功能时修改功能。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 为可选项，但可以为代理提供清晰的显示名称。
3. 阅读配置文件说明，确认 QA 从需求出发，遵循仓库说明，运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供支持证据，在更改实现代码前征求同意，并且不会提交更改或打开 PR。
5. 如果配置文件遗漏任何职责或边界，请在继续之前要求默认代理修改。

## 根据 issue 运行 QA

Copilot CLI 会在对话启动时加载项目代理。创建配置文件后，恢复同一个筛选对话并选择 QA，以便它使用已有的 issue 和规划决定。

1. 使用以下命令启用代理：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 由于刚刚创建了代理，它可能不会显示在可用代理列表中。代理已存在，上述命令会启用它。

2. 使用以下提示，请 QA 代理检查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 观察 QA 代理开始工作。
4. 工作完成后，阅读它提供的报告。

## 总结和后续步骤

你已将可复用的专业角色添加到工作流中，并检查了它的工作。在本课程中，你：

- 探索了自定义代理如何结合说明、技能和 MCP 工具。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 代理，并根据筛选 issue 检查了它的发现。

现在，实现、技能更新、QA 配置文件、测试和验证报告都已准备好供审查。接下来，[将它们汇总到功能 PR 中并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义代理][create-agents]
- [自定义代理配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
