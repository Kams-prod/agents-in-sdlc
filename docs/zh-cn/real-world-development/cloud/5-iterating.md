---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作

在本实验中，您使用 GitHub Copilot 完成了多个改善用户体验并添加功能的任务。您请 Copilot 为代码添加文档、构建设计团队可继续迭代的相关游戏功能，并实现高对比度和浅色模式切换等无障碍功能。现在检查代码更改，并在必要时向 Copilot 提供反馈，改进其工作。

### 场景

如前所述，添加生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。因此，在为团队其他成员启用审查之前，请先检查 GitHub Copilot 为代码文档、相关游戏功能和无障碍功能生成的建议。

## 安全与 GitHub Copilot 云智能体

由于 Copilot 云智能体会异步且无人监督地执行任务，因此设置了一些安全限制，确保工作安全：

- Copilot 对仓库只有读取权限，并且**只有**对其代码分支具有写入权限。
- 云智能体在 GitHub Actions 中运行，并在其中创建独立的临时环境来工作。
- 任何 GitHub Actions 工作流都必须经过人工批准后才能运行。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，包括 MCP 服务器。

## 审查生成的文档

先查看 GitHub Copilot 云智能体创建的第一个拉取请求 (PR)——为代码添加文档。您将使用 GitHub.com 标准 PR 界面完成这项工作。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期情况**，因为 Copilot 默认只能有限访问外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体防火墙][agent-firewall]。

1. 返回 github.com 上的仓库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会显示 **[WIP]** 标记。请等待 Copilot 完成工作。可能需要几分钟，可以趁此休息或回顾所学内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 检查更新后的代码，其中包含新生成的 TSDoc 文档注释和其他文档。具体更改会有所不同。

    检查时，请留意 TSDoc 文档注释、TypeScript 约定和文件注释标题。这些要求来自您在练习 2 开始时查看的自定义指令文件。

6. 审查完更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 您应该会看到工作流正在等待批准的指示。
8. 如果工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 您应该会看到工作流已在拉取请求的检查区域中排队。如果一切顺利，单个 Astro 应用的项目检查将通过。可能需要几分钟。

## 请 GitHub Copilot 修改工作

与 Copilot 协作并非单向过程。您可以像标记团队成员一样，在 PR 评论或代码行内评论中标记 Copilot。Copilot 会读取这些评论，并启动新会话处理反馈。由于结果具有不确定性，我们无法提供固定提示文本。可以请 Copilot 做以下更新：

- 在每个代码文件顶部添加简要说明文件用途的注释标题。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，说明 Astro 应用结构。

1. 添加评论，请求修改生成的文档，并像标记其他用户一样标记 **@copilot**。可以使用以上任一建议，也可以提出希望代码库文档包含的其他内容。
2. 选择 **View Session**，观察 Copilot 工作。注意 Copilot 会启动新会话来进行更新。
3. 选择 **Back to pull request** 可返回拉取请求。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中应该会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

可以继续迭代，直到满意为止。确认满意后，可将 PR 从草稿转换为准备审查状态，并合并到主分支。

![将 PR 转换为准备审查状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在查看 Copilot 为解决“在游戏详情页显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
6. 您应该会看到工作流正在等待批准的指示。
7. 如果工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 您应该会看到工作流已在拉取请求的检查区域中排队。如果一切顺利，单个 Astro 应用的项目检查将通过。可能需要几分钟。
9. **可选：**可以在 Codespace 中切换到该分支，手动测试相关游戏功能。进入 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已完成相关游戏功能！与之前一样，您可以通过迭代请 Copilot 修改。例如，请它调整显示的相关游戏数量，或添加注释标题和 TSDoc 文档注释（请记住——分配此议题时，您尚未更新自定义指令！）。与之前一样，在 **Conversation** 选项卡中添加评论即可提出要求；Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍访问自定义智能体实现的功能。此 PR 应包含练习 3 中分配的高对比度模式，以及练习 4 中在任务控制中心提出的浅色模式。

1. 返回 GitHub.com 上的仓库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会显示 **[WIP]** 标记。请等待 Copilot 完成；可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，特别留意：
   - 用于切换模式的 UI 组件
   - 用于保存用户偏好的本地存储
   - 高对比度和浅色模式的 CSS 或样式更改
   - 无障碍属性（ARIA 标签、键盘导航等）
   - 管理模式切换的 JavaScript/TypeScript 代码

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 您应该会看到工作流正在等待批准的指示。
8. 如果工作流等待批准，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 您应该会看到工作流已在拉取请求的检查区域中排队。如果一切顺利，单个 Astro 应用的项目检查将通过。可能需要几分钟。
10. **可选：**可以在 Codespace 中切换到该分支，手动测试无障碍功能。进入 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，在浏览器中测试高对比度和浅色模式切换，确认功能正常，并且刷新页面后设置仍然保留。

请注意，无障碍访问自定义智能体如何引导 Copilot 遵循无障碍最佳实践实现这些功能。如果发现无障碍问题或改进空间，可以像对待之前的 PR 一样，在评论中标记 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

当您可以将实际待办事项交给云智能体处理，并转而专注于其他工作时，云智能体最有用。可以通过向仓库添加几个议题并分配给 Copilot 来培养这种习惯。以下是一些想法：

- 在游戏详情页创建支持者意向表单。
- 为游戏列表页添加分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

您已完成云智能体开发环境的练习。通过这些课程，您：

- **检查了仓库随附的自定义指令文件**，以了解它们对云智能体后续输出的影响。
- **将议题分配给 Copilot 云智能体**，观察它设置环境、规划并异步执行任务。
- **创建并使用了自定义智能体**，为无障碍访问添加高对比度和浅色模式切换。
- **使用 Copilot 智能体页面作为任务控制中心**，监控并引导正在进行的无障碍访问会话。
- **审查并迭代改进了云智能体的拉取请求**，使用 `@copilot` 请求更改并批准工作流。

## 审查和后续步骤

您已完成云智能体开发环境的练习。如果希望继续探索，其他开发环境的课程可以补充您在此处学到的内容：

- 🖥️ **[VS Code 开发环境](../../vscode/)** — 直接在 IDE 中探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 开发环境](../../cli/)** — 在终端中交付两个经过审查的更改，体验 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接功能。

在自己的仓库中，可以尝试以下后续练习：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
