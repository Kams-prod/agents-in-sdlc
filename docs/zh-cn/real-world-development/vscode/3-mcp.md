---
title: "练习 3 - 使用 Playwright MCP 服务器测试功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：智能体模式][previous-lesson] · [下一课：自定义智能体 →][next-lesson]

您刚刚使用智能体模式构建了筛选功能。创建拉取请求之前，应先确认它能在浏览器中正常运行。您无需亲自逐项点击应用，而是连接 **Playwright MCP 服务器**，让 Copilot 驱动真实浏览器测试功能，然后发布分支并创建 PR。

在本练习中，您将：

- 了解模型上下文协议 (MCP) 以及 MCP 服务器如何扩展 Copilot 的工具；
- 将 Playwright MCP 服务器添加到工作区；
- 请 Copilot 使用该服务器，在浏览器中手动测试筛选功能；
- 发布分支，并为筛选工作创建拉取请求。

## 什么是模型上下文协议 (MCP)？

当智能体模式能够访问编辑器之外的资源时，功能会更加强大。Copilot 使用模型上下文协议 (MCP) 与外部工具和服务进行通信，这是智能体与这些工具和服务交互的标准方式。

![示意图：智能体模式内部工作方式，以及它如何与上下文、LLM 和工具交互，包括由 MCP 服务器和 VS Code 扩展提供的工具](../../../_images/ex1-mcp-diagram.png)

[模型上下文协议 (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/)使 AI 智能体能够与外部工具和服务通信。借助 MCP，AI 智能体可以实时访问最新信息（通过资源），并代您执行操作（通过工具）。

这些工具和资源通过 MCP 服务器访问。MCP 服务器充当 AI 智能体与外部工具及服务之间的桥梁，并负责管理智能体与外部工具（例如现有 API 或 npm 包等本地工具）的通信。每个 MCP 服务器都提供一组可供 AI 智能体使用的工具和资源。

以下是两个常见的 MCP 服务器：

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**：提供用于管理 GitHub 仓库的 API，让 AI 智能体可以创建新仓库、更新现有仓库，以及管理议题和拉取请求。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**：使用 Playwright 提供浏览器自动化功能，让 AI 智能体可以浏览网页、填写表单和选择按钮。

还有许多其他 MCP 服务器，可以提供对不同工具和资源的访问。GitHub 提供了 [MCP 注册表](https://github.com/mcp)，方便发现和参与生态系统。

> [!CAUTION]
> 从安全角度来看，对待 MCP 服务器应像对待项目中的其他依赖项一样。使用前请仔细审查源代码、验证发布者并考虑安全影响。只使用您信任的 MCP 服务器，并谨慎授予其访问敏感资源或执行操作的权限。

## 查看 MCP 配置

`.vscode/mcp.json` 文件配置了此 VS Code 工作区中可用的 MCP 服务器。

1. 在 codespace 中打开 `.vscode/mcp.json`。
2. 您应该会看到已配置的 `github` 服务器：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

项目模板中已包含此 `github` 条目，因此 Copilot 才能在上一个练习中读取议题待办列表。它使用[远程 GitHub MCP 服务器][remote-github-mcp-server]，无需在本地安装；VS Code 通过 HTTP 连接，Copilot 首次使用相关工具时，您需要使用 GitHub 进行身份验证。

## 添加 Playwright MCP 服务器

现在添加第二个服务器。[Playwright MCP 服务器][playwright-mcp-server]会为 Copilot 提供可控的浏览器，正好可以用来测试功能。

1. 在 `.vscode/mcp.json` 中添加 `playwright` 条目，与 `github` 并列，使文件内容如下：

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. 保存文件。

与远程 `github` 服务器不同，`playwright` 是**本地**服务器：VS Code 会运行 `npx @playwright/mcp@latest` 启动它。`--headless` 标记指示 Playwright 在没有可见窗口的情况下运行浏览器；codespace 中没有可显示窗口的桌面环境，因此必须使用此选项。

> [!NOTE]
> Tailspin Toys 项目已经使用 Playwright 进行端到端测试，因此通常已安装浏览器。如果 Copilot 之后报告缺少浏览器，请让它运行 `npx playwright install chromium`，然后重试。

## 启动并信任服务器

VS Code 会**按需**启动 MCP 服务器——Copilot 首次需要使用服务器工具时，VS Code 会启动服务器并询问您是否信任它。

1. 在 `.vscode/mcp.json` 中，`playwright` 条目上方会显示 **Start** 操作。选择它立即启动服务器，并确认配置正确。
2. 如果 VS Code 询问是否信任该服务器，请审查配置并选择信任，以便服务器启动。

手动启动并非必需。如果跳过此步骤，VS Code 会在下一步中 Copilot 首次需要服务器工具时启动它，并询问您是否信任。

## 测试筛选功能

[Playwright MCP 服务器][playwright-mcp-server]可以为 Copilot 提供可操作的真实浏览器。无需您逐项点击应用来检查结果，智能体可以打开页面、导航、应用筛选条件、读取结果并总结观察情况。这是在不离开对话的情况下快速确认功能符合预期的方法。

Playwright MCP 服务器使用网页的[无障碍树][playwright-mcp-server]，而不是截图。这意味着智能体会像辅助技术一样，根据带标签的结构化元素（按钮、链接和列表项）进行推理。因此，快速功能检查也能顺便检查基本的无障碍情况。

连接服务器并启动应用后，请 Copilot 测试刚构建的筛选功能：

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot 会通过 Playwright MCP 服务器启动浏览器、逐项执行测试并报告结果。请根据议题中的验收标准审查报告；如果发现问题，请追问或要求 Copilot 修复代码，再创建拉取请求。

> [!NOTE]
> 执行此测试时，应用必须运行在 `http://localhost:4321`。如果已停止开发服务器，请在发送提示前重新启动。如果 Copilot 首次使用 Playwright MCP 服务器时需要下载浏览器，可能会报告缺少浏览器；让它运行 `npx playwright install chromium`，然后重试。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## 发布分支并创建拉取请求

确认功能正常后，就可以创建拉取请求 (PR)，供团队审查。首先发布 `filtering-vscode` 分支。

1. 打开 codespace 中的 **Source Control** 面板，审查 Copilot 做出的更改。
2. 选择 **+** 图标暂存更改。
3. 使用 **Sparkle** 按钮生成提交消息。

   ![显示 Copilot 所做更改的 Source Control 面板截图](../../../_images/ex3-source-control-changes.png)

4. 选择 **Publish**，将分支推送到仓库。

## 创建拉取请求

您可以在 github.com 或通过 GitHub 命令行界面 (CLI) 创建 PR。既然已在使用 GitHub Copilot，不妨让它帮忙创建！它可以找到相关议题并创建与其关联的 PR。

1. 打开 Copilot Chat 面板并选择 **New Chat**，启动新会话。
2. 确认智能体下拉菜单中已选择 **Agent**，使 Copilot 能够使用 GitHub 工具。
3. 请 Copilot 为您创建 PR：

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 根据需要选择 **Continue**，允许 Copilot 执行查找信息和操作所需的任务。首次使用 GitHub 工具时，系统可能会提示您登录 GitHub；按提示完成授权。
5. 注意 Copilot 如何搜索议题、找到相关议题并创建 PR。
6. 选择 Copilot 生成的链接，审查拉取请求，但**不要立即合并**。

## 总结和后续步骤

恭喜！在本练习中，您：

- 了解了模型上下文协议 (MCP) 以及 MCP 服务器如何扩展 Copilot 的工具；
- 将 Playwright MCP 服务器添加到了工作区；
- 使用它在发布前通过浏览器手动测试了筛选功能；
- 发布了分支，并为筛选工作创建了拉取请求。

您使用 MCP 服务器测试了功能，但 MCP 只是扩展 Copilot 的一种方式。接下来，[创建自定义智能体][next-lesson]，简化无障碍审查等专项任务。

## 资源

- [MCP 是什么，为什么大家都在讨论它？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP 注册表][mcp-registry]
- [VS Code 中的 MCP 服务器][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
