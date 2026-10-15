---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一分支中。现在可以一起审查，并创建拉取请求。星级评分拉取请求 (PR) 是您手动合并的；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 通常会将功能、指令更新、技能更新和 QA 智能体分别放在几个 PR 中。为了简化本研讨会，我们在同一对话和分支中完成了完整的筛选与质量工作流，并将所有更改放入此 PR。

在本课中，您将：

- 了解 Agent Merge 及其如何自动管理合并流程。
- 检查完整的功能更改和验证证据。
- 创建筛选功能 PR。
- 完成审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，您使用 Copilot 规划、实现并验证了功能。Tailspin Toys 现在希望自动完成 PR 的剩余工作，同时由开发人员掌控合并授权。

## 介绍 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理阻碍 PR 合并的事项——修复持续集成 (CI) 检查失败、响应审查意见，并在需要时变基——然后在仓库允许的情况下启用 GitHub 自动合并。

此前，您是亲自选择 **合并拉取请求**。Agent Merge 可以代您完成这项工作。启用 Agent Merge 前，先审查更改并确认其已准备就绪。

## 使用 Agent Merge 管理 PR

所有代码都已完成，现在一起审查、创建 PR，并让 Agent Merge 管理后续流程。

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项列表中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 使用命令 `/pr create` 创建 PR。
5. 使用 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 将监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认看到 Copilot 的消息，内容类似“PR #14 was squash-merged successfully.”

> [!IMPORTANT]
> Agent Merge 不会绕过必需的审批、分支保护、合并队列、仓库设置或权限限制。如果流程受阻，请查看报告的原因；在仓库允许的情况下，完成人工审查后的合并。

## 总结和后续步骤

您已自动化开发流程中的多个环节，包括生成代码、测试和验证代码，以及管理拉取请求。在本课中，您：

- 了解了 Agent Merge 及其如何自动管理合并流程。
- 检查了完整的功能更改和验证证据。
- 创建了筛选功能 PR。
- 在审查后启用了 Agent Merge，并确认 PR 已合并。

接下来，您将[探索更多实用的 Copilot CLI 斜杠命令][next-lesson]，以管理上下文、模型和共享，并了解可选的云委派功能。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
