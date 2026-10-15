---
title: "第 7 课 - 创建和使用 QA 智能体"
description: "创建 QA 自定义智能体，将议题需求、quality-checks 技能和 Playwright MCP 结合起来。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

您已使用 `quality-checks` 技能运行自动检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在，将这些能力整合到自定义智能体中，并为其定义清晰的 QA 流程。

在本课中，您将：

- 了解自定义智能体如何配合指令、技能和 MCP 工具工作。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，并根据筛选议题审查其结论。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，统一检查需求、代码质量、自动检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程，并生成可重复使用的报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件描述智能体的用途、指令和可用工具。在本研讨会中，您将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

本研讨会中使用的自定义功能各有不同用途：仓库指令描述团队标准，`quality-checks` 技能封装可重复执行的检查，Playwright MCP 提供浏览器工具，而 QA 配置文件则指导 Copilot 如何使用这些能力评估需求并报告发现。它不会取代这些功能，也不需要启动另一个对话。

## 创建 QA 配置文件

创建功能 PR 前，请 Copilot 创建可复用的 QA 配置文件。该配置文件将定义 QA 执行的检查及其必须遵守的限制。

1. 返回 Codespace，并确认筛选对话处于 Interactive 模式。
2. 请默认智能体创建新的自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，请先检查其配置文件，确认 Copilot 按预期记录了 QA 流程和限制。这样可避免配置不完整或权限过宽的智能体在您只想验证功能时修改实现。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 为必填项；`name` 是可选项，但添加后可作为智能体的显示名称。
3. 阅读配置文件中的指令，确认 QA 会从需求出发、遵循仓库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会报告支持证据、修改实现代码前会先询问，并且不会提交更改或打开拉取请求。
5. 如果生成的配置文件遗漏了这些职责或限制，请让默认智能体修改后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后，恢复同一筛选对话，再选择 QA，使它可以使用已有的议题和规划决策。

1. 使用以下提示启用该智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能暂时不会出现在可用智能体列表中，但仍可通过上述命令启用。

2. 使用以下提示让 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA 智能体将开始工作。
4. 完成后，阅读它生成的报告。

## 总结和后续步骤

您已在工作流中添加可复用的专家角色，并审查了它的工作。在本课中，您：

- 了解了自定义智能体如何配合指令、技能和 MCP 工具工作。
- 创建并检查了可复用的质量保证配置文件。
- 选择了 QA 智能体，并根据筛选议题审查其结论。

现在，您已准备好实现、技能更新、QA 配置文件、测试和验证报告。接下来，将它们[整合到功能 PR 中，并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
