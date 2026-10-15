---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中检查或配置 Playwright MCP，并用它在浏览器中试用筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

如前所述，编写代码不只是写代码。我们还需要处理数据、外部服务，并让 Copilot 使用更多自动化工具。MCP 服务器正是为此而生。它们能让 Copilot 超越 CLI 内置功能，使用更多工具和服务。

在本课中，您将：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 在尚未配置 Playwright MCP 服务器时添加该服务器。
- 请智能体驱动浏览器并试用筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际与界面交互。您希望 Copilot 像用户一样使用网站，进一步自动化更改验证流程，并增强对更新符合预期的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 智能体提供了与外部工具和服务通信的方式。借助 MCP，AI 智能体可以实时访问最新信息，并代您执行操作。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具及服务之间的桥梁。每个 MCP 服务器都提供一组可供 AI 智能体使用的工具和资源。

以下是两个常见的 MCP 服务器：

- **[GitHub MCP Server][github-mcp]**：提供用于管理 GitHub 仓库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，以便更轻松地发现和参与生态系统。

> [!CAUTION]
> 对待 MCP 服务器应像对待项目中的其他依赖项一样。使用前请审查源代码、验证发布者，并考虑安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 中已配置 Playwright MCP，请跳过这些设置步骤，继续下一部分。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 名称输入 `playwright`，然后按 <kbd>Tab</kbd>。
4. 按 <kbd>Enter</kbd> 确认服务器类型为 **STDIO**，然后按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴以下内容：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 请 Copilot 使用 Playwright 试用功能

此前，您已手动确认功能符合预期。现在请使用 Copilot 中的 Playwright MCP 服务器完成同样的验证！

1. 使用以下提示，请 Copilot 通过 Playwright MCP 服务器验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确要求 Copilot 使用 MCP 服务器，因为它往往会自行判断。但既然您知道应该使用哪个工具，明确指出也有助于它更稳定地得出结果，还能节省一些 token！

2. 观察 Copilot 如何列出其在浏览器中执行的各项步骤，以确认功能正常。
3. 阅读报告，确认一切符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并向您提供报告。

## 总结和后续步骤

恭喜！您已在 Copilot CLI 中使用 Playwright MCP 服务器，通过真实浏览器试用了功能。回顾一下，您：

- 了解了模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 在 Playwright MCP 服务器尚未配置时添加了它。
- 请智能体驱动浏览器并试用了筛选功能。

接下来，您将[创建 QA 自定义智能体][next-lesson]，将技能和浏览器工具整合到专用角色中。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [将 MCP 服务器添加到 Copilot CLI][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
