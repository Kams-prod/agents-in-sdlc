---
title: "第 2 课 - 添加星级评分：快速上手"
description: "使用 Copilot CLI 修改游戏卡片，在转发的浏览器中审查更改，并将其作为您的第一个拉取请求合并。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

安装 Copilot CLI 并尝试过对话后，现在可以首次修改项目了。我们从一个小改动开始：游戏数据中已有星级评分，但首页的游戏卡片尚未显示评分。您将请智能体显示评分、审查更改，并将其作为第一个拉取请求合并。

在本课中，您将：

- 在功能分支上启动一个专注于单项任务的 Copilot 对话。
- 请智能体对项目进行小幅修改。
- 使用 `/diff` 审查更改。
- 运行应用，在转发的浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，游戏详情页已经显示评分，但首页的游戏卡片目前只显示标题、类别、发行商和说明。作为热身练习，请智能体在每张卡片上显示已有评分——这是一个范围明确的小改动，非常适合第一次会话。

## 对话的组成部分

**对话**是您在 Copilot CLI 中处理任务的地方。与 Copilot app 不同，普通 CLI 对话会使用终端中当前检出的仓库和 Git 分支，而不是创建专用工作树。保存的对话可让您稍后返回同一讨论，而文件和分支仍是磁盘上的普通 Git 状态。

在对话中，您会看到三类内容：您发送的提示和智能体的回复、智能体探索和编辑文件时执行的工具操作，以及可通过 `/diff` 检查的更改。

## 启动对话并请求更改

启动一个新对话来实现此功能。

1. 返回 Codespace。
2. 如果终端尚未打开，请按 <kbd>Control</kbd>+<kbd>\`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>\`</kbd>（Windows/Linux）。
3. 如果 Copilot 尚未启动，请运行以下命令：

   ```bash
   copilot --yolo
   ```

4. 使用斜杠命令 `/new` 并按 <kbd>Enter</kbd>，确保启动新会话。
5. 使用以下提示请求更改：

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot 会探索项目，找到显示游戏详情所用的文件，并生成所需代码。您已使用 Copilot CLI 添加了一个新功能！

## 审查差异

所有 AI 生成的更改都应在合并前审查，即使改动很小也不例外。现在就在 Copilot CLI 中检查更改。

1. 输入 `/diff` 并检查每个已更改的文件。
2. 确认游戏卡片会在有评分时显示数字评分，在 `starRating` 为 `null` 时显示 `No rating yet`。
3. 确认测试覆盖这两种情况。
4. 审查 Copilot 运行检查的结果，并请它修复任何失败项。
5. 完成审查后，按 <kbd>Esc</kbd> 退出差异视图。

> [!NOTE]
> 与所有生成式 AI 工具一样，Copilot 的输出具有概率性而非确定性，因此具体代码可能有所不同。请审查实际行为，不要期待实现必须与某个示例完全一致。

## 检查更改

当然，不能只看代码就假定它能正常运行。请让 Copilot 启动网站，然后在 Codespaces 转发的浏览器中检查更新后的界面。

1. 请 Copilot 启动应用：

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **在浏览器中打开**。
3. 确认游戏卡片会显示五星制评分。
4. 返回 Copilot，并请它停止启动的服务器：

   ```plaintext
   Stop the development server you started.
   ```

## 创建并合并第一个拉取请求

您已完成该功能，现在可以创建拉取请求（PR），将新代码合并到项目中。

1. 请默认智能体提交更改：

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. 输入 `/pr create`。Copilot CLI 创建 PR 时会推送现有提交，并显示 PR URL。
3. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Copilot CLI 显示的 URL，以打开 PR。
4. 审查已更改的文件和检查结果。
5. 准备就绪后，选择 **合并拉取请求**，然后确认合并。
6. 返回 Codespace，并使用 `/exit` 退出 Copilot CLI。
7. 更新本地 `main` 分支：

   ```bash
   git checkout main
   git pull
   ```

## 总结和后续步骤

恭喜！您已使用 GitHub Copilot CLI 发布了第一个更改。具体来说，您：

- 在功能分支上启动了一个专注于单项任务的 Copilot 对话。
- 引导智能体对游戏卡片进行小幅修改。
- 使用 `/diff` 审查了更改。
- 运行应用，在转发的浏览器中确认星级评分。
- 创建并合并了第一个拉取请求。

接下来，您将[从筛选议题开始，使用 Plan 和 Autopilot 模式][next-lesson]构建更大的功能。

## 资源

- [关于 GitHub Copilot CLI][about-copilot-cli]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
