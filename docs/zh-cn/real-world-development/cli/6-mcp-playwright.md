---
title: "课程 6 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并使用它在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

正如前面强调的，编写代码不只是写代码。我们需要使用数据和外部服务，有时还要为 Copilot 提供额外的自动化能力。Model Context Protocol (MCP) 服务器可以帮助实现这一点，让 Copilot 超越 CLI 的内置功能，访问更多工具和服务。

在本课程中，你将：

- 了解什么是 Model Context Protocol (MCP)，以及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚不可用，则添加该服务器。
- 请求代理控制浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证用户界面 (UI) 更改需要与界面交互。你希望 Copilot 像用户一样使用正在开发的网站，从而进一步实现自动化，并增强对功能符合预期的信心。

## 什么是 Model Context Protocol (MCP)？

[Model Context Protocol (MCP)][mcp-blog-post] 允许 AI 代理与外部工具和服务实时通信。因此，代理可以访问最新信息，并代表你执行操作。

工具和资源通过 MCP 服务器提供。服务器充当 AI 代理与外部工具和服务之间的桥梁。每个服务器都会向代理提供不同的工具和资源。

以下是两个常用的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 仓库、issue 和拉取请求的 API 访问权限。
- **[Playwright MCP Server][playwright-mcp-server]**：通过 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器可用。GitHub 托管了一个 [MCP 注册表][mcp-registry]，方便用户发现服务器并为生态系统做出贡献。

> [!CAUTION]
> 将 MCP 服务器视为项目的其他依赖项。使用之前，请查看源代码、核实发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果 Playwright MCP 服务器尚不可用，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些步骤并继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入此命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入 `playwright` 作为名称，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 将以下命令粘贴到 **Command** 对话框中：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd> (Mac) 或 <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) 保存 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请求 Copilot 使用 Playwright 探索功能

你已手动检查功能，确认其行为符合预期。现在，使用 Copilot 可用的 Playwright MCP 服务器进行同样的检查。

1. 使用此提示，请求 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 无需明确要求 Copilot 使用 MCP 服务器：它通常能推断应该使用哪个服务器。不过，既然你知道要使用哪个服务器，明确指定可以提高结果的一致性并节省一些 token。

2. 观察 Copilot 在浏览器中检查功能时显示的步骤。
3. 阅读报告并确认所有内容都按预期工作。

Copilot 将启动服务器，通过 Playwright 与网站交互，停止服务器，然后提供报告。

## 总结和后续步骤

你已在 Copilot CLI 中使用 Playwright MCP 服务器，通过真实浏览器探索功能。本课程中，你：

- 了解了什么是 Model Context Protocol (MCP)，以及 Copilot CLI 如何使用它。
- 如果 Playwright MCP 服务器尚不可用，则添加了该服务器。
- 请求代理控制浏览器并探索了筛选功能。

接下来，[创建自定义 QA 代理][next-lesson]，将技能与浏览器工具结合成专门的角色。

## 资源

- [什么是 MCP，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP 服务器][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
