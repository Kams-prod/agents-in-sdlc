---
title: "练习 6 - 迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
next: false
---

| [← 上一课：监控和管理智能体][previous-lesson] |
| :-- |

## 审查工作

在本实验中，您使用 GitHub Copilot 完成了多项改善用户体验的任务。您使用智能体模式在客户端和服务器端添加筛选功能，通过 Playwright MCP 服务器在浏览器中手动测试，然后使用自定义智能体实现高对比度和浅色模式切换等无障碍功能，并在会话进行中引导它扩展工作。现在可以发布这些本地更改，并像团队审查工作一样检查结果。

### 场景

引入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们仍需对 Copilot 生成的内容进行完整审查。因此，现在将 codespace 中的无障碍功能更改推送到分支、创建拉取请求，并在邀请其他团队成员参与之前检查差异。

## 发布无障碍功能

您在[练习 4][exercise-4]和[练习 5][exercise-5]中使用无障碍自定义智能体实现的高对比度和浅色模式切换，已作为提交保存在 codespace 中。现在将其推送到分支并创建拉取请求，供团队其他成员审查。

1. 返回 codespace。
2. 在 VS Code 中打开 **Source Control** 视图。
3. 确认无障碍功能更改已提交。如果练习 5 中仍有未提交的更改，请暂存并提交，可使用说明性提交消息，例如 `Add high-contrast and light-mode toggles`。
4. 选择 **Publish Branch** 发布分支（或使用 **...** 菜单 → **Push**）。
5. VS Code 会提示您在 github.com 上打开新分支。接受提示，或手动访问仓库并在分支横幅中选择 **Compare & pull request**。
6. 设置清晰的标题（例如 `Add high-contrast and light-mode toggles`），并简要说明完成了哪些工作以及原因。
7. 选择 **Create pull request**。
8. 打开 PR 后，选择 **Files changed** 选项卡，完整审查更改。重点关注：
   - 用于切换模式的 UI 组件。
   - 用于持久保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

9. 返回 **Conversation** 选项卡。
10. 如果工作流等待批准，请选择 **Approve and run workflows**。

    ![批准并运行工作流](../../../_images/shared-approve-workflows.png)

11. 等待工作流完成。如果一切顺利，所有工作流都会通过。

> [!TIP]
> 想为无障碍功能再获取一份意见？在 PR 评论中标记 `@copilot`，并提出类似“审查此 PR 是否还存在其他 WCAG 问题”或“建议改进键盘导航”的请求。Copilot 会启动新会话处理评论。

## 可选练习 - 继续在本地探索

在 IDE 中与智能体迭代协作是一项技能，只有反复练习才能掌握。以下是一些可以在 VS Code 中启动的后续会话想法：

- 在游戏详情页添加支持者意向表单。
- 在游戏列表页实现分页。
- 为 `src/lib/` 中的数据访问辅助函数添加输入验证和错误处理。
- 扩展无障碍智能体的工作范围，例如审查整个网站的键盘焦点顺序。

## 总结

恭喜！您已完成 VS Code 开发环境的练习。在本实验中，您：

- **使用 Playwright MCP 手动测试了功能。** 添加 Playwright MCP 服务器，让 Copilot 操作浏览器，在创建拉取请求前验证筛选功能。
- **使用智能体模式协调跨技术栈的更改。** 在同一会话中添加了涉及客户端、服务器和测试的筛选功能。
- **使用了自定义智能体。** 从智能体选择器中选择无障碍访问自定义智能体，观察它如何针对仓库实现高对比度模式。
- **管理并引导了智能体会话。** 在聊天中内联审查建议的更改，接受所需更改，并通过后续提示扩展会话，添加浅色模式。
- **通过拉取请求完成工作闭环。** 发布了本地更改，并像团队一样完整审查结果。

## 审查和后续步骤

VS Code 开发环境的必修部分到此结束。您可以在这里停下，研讨会已完成。

如果希望从其他角度了解 Copilot 的智能体功能，其他开发环境会通过不同界面介绍相关场景：

- 💻 **[CLI 开发环境](../../cli/)** — 在终端中交付两个经过审查的更改，体验 Plan 和 Autopilot 模式、技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及基于上下文的工作交接。
- ☁️ **[云智能体开发环境](../../cloud/)** — 了解如何将议题分配给云智能体、通过智能体页面监控会话，并异步迭代改进拉取请求。

您还可以继续扩展当前成果。[awesome-copilot][awesome-copilot]提供了丰富的指令文件、自定义智能体和技能，可根据项目需要进行调整。

若要进行可选扩展，[可选：集成 Foundry][exercise-7]将使用 VS Code 和 Microsoft Foundry Toolkit 准备模型、部署 Backer Concierge，并将其连接到网站。

## 资源

- [GitHub Copilot][github-copilot]
- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[previous-lesson]: ../5-managing-agents/
[exercise-4]: ../4-custom-agents/
[exercise-5]: ../5-managing-agents/
[exercise-7]: ../7-foundry-toolkit/
[github-copilot]: https://github.com/features/copilot
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
[awesome-copilot]: https://github.com/github/awesome-copilot
