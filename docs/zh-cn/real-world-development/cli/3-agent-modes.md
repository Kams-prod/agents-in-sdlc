---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 确认实现方式，使用 Autopilot 根据议题实现筛选功能，并通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们刚刚为项目添加了一项小功能。但较大的更改需要更完善的流程。幸运的是，GitHub Copilot CLI 可以配合组织现有的工作流程，确保我们以正确的方式构建正确的内容。本课是系列课程的第一课，您将遵循典型的智能体驱动开发流程：从议题着手实现新功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能工作将继续使用同一对话和分支。通常，不同类型的文件会分别使用不同分支或 PR；为了专注于核心概念，本研讨会会采用简化方式。

本课将介绍如何：

- 从 GitHub 议题启动新的 Copilot 对话。
- 在 Plan 模式中确定需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

继续开发此功能时，您还将更新仓库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体，并打开功能 PR。

## 场景

Tailspin Toys 的游戏目录不断扩大，访客需要按类别和发行商筛选游戏。待办事项中的议题描述了这一功能，但组合类别等细节需要在编码前达成一致。您将使用 Plan 模式解决这些问题，然后授权 Autopilot 在限定范围内实现。

## 背景

将 AI 编码智能体引入开发流程，并不会改变开发的基本原则。事实上，这些原则变得更加重要！大多数开发人员遵循的流程大致如下：

1. 从已创建的议题着手，明确要完成的工作。
2. 制定实现方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求（PR）。
7. 代码经过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多与上述流程类似。

坚持这种标准做法，可以确保 AI 生成的代码满足既定需求，并经过与手写代码相同的审核流程。

## 对话模式

**对话模式**决定智能体拥有多少自主权。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式之间切换：

- **Interactive**：您与智能体协同工作。智能体提出更改建议，并等待您的输入后再继续。
- **Plan**：智能体先制定计划，在此模式下无法编辑项目文件。
- **Autopilot**：智能体自主工作——编写代码、运行测试，并反复迭代，直到任务完成。

先从 Plan 模式开始，审查计划，然后使用 Autopilot 实现计划。

## 从议题开始

开始筛选工作前，请返回 Codespace，确认仓库和终端已准备就绪。

1. 返回 Codespace。如果它已停止，请先重新启动。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
4. 更新 `main`，然后为筛选工作创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用箭头键选中标题为 **Allow users to filter games by category and publisher** 的议题。
9. 按 <kbd>C</kbd> 将议题添加到提示中，然后返回 **Session** 选项卡。

请注意，提示现在以 `#7`（或类似编号）开头。`#` 可将 GitHub 上的议题或拉取请求（PR）添加到上下文中。

## 规划筛选功能

规划能让您在交由 Copilot 实现功能或执行任务前，先确定实现方法。对于复杂任务，花些时间制定计划总是有益的。现在切换到 Plan 模式，让 Copilot 制定计划。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式。确认提示下方的模式指示器显示 **Plan**。
2. 在上一步添加的议题引用后输入以下提示：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 将开始制定计划！它会先探索项目，再确定最佳实现方式。

3. Copilot 可能会询问筛选功能的工作方式。根据您的偏好回答；没有错误答案！
4. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
5. 滚动查看并审查计划。
6. 如果计划的任何部分不符合您的决定，请让 Copilot 修改。

## 批准 Autopilot

计划已编写并审查完毕，现在可以开始实现！使用 Autopilot 让 Copilot 执行任务。

Autopilot 会让 Copilot 反复处理问题，直到它认为任务已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或您安装的版本中标签相似的选项。
2. 确认提示下方的模式指示器显示 **Autopilot**。
3. 观察 Copilot 如何遵循已确认的计划、生成代码并运行测试。

> [!NOTE]
> 批准后可能会立即开始实现，因此请先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，请先解决设置问题，再将检查视为已完成。

## 审查并验证实现

代码生成后，应像其他代码一样在合并前进行审查。现在审查代码并运行网站，确认一切正常。

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Interactive 模式。确认模式指示器不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选实现和测试。
3. 对照议题和规划时做出的决定检查结果。
4. 审查代码后，按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查的输出，并请 Copilot 修复任何失败项。

## 试用新功能

代码看起来没问题了——但它能运行吗？像之前一样启动应用，通过 Codespaces 转发端口打开网站。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **在浏览器中打开**。
3. 尝试按类别筛选、按发行商筛选，以及按照计划中商定的方式组合筛选条件。
4. 确认重置和无结果时的行为符合议题要求和您的决定。
5. 返回 Codespace，请 Copilot 停止它启动的开发服务器。

## 总结和后续步骤

您已使用不同的对话模式构建并审查功能。在本课中，您：

- 从 GitHub 议题启动了新的 Copilot 对话。
- 在 Plan 模式中确定了需求。
- 使用 Autopilot 模式实现了新功能。
- 审查了代码。
- 在转发的浏览器中手动验证了功能。

接下来，您将[使用自定义指令][next-lesson]进一步了解如何确保代码遵循文档化的实践。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
