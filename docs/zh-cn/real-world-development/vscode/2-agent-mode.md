---
title: "练习 2 - 使用 Copilot 智能体模式添加新功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是最简单的应用更新，通常也需要修改多个文件并执行一些操作，例如运行测试。开发人员通常需要找到所有相关文件、进行更改、运行测试、调试、找出遗漏的文件并再次修改……还有很多类似步骤。

这正是 Copilot 智能体模式发挥作用的地方。

Copilot 智能体模式可在 IDE 中自主工作。它的行为类似开发人员：先探索现有项目结构，再执行必要的更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用智能体模式为网站添加新功能。

> [!NOTE]
> 智能体模式和云智能体名称相似，但提供的是两种不同体验。智能体模式在 IDE 中执行任务，可快速反馈和交互。云智能体则像团队成员一样异步工作，通过议题和拉取请求与您协作。

在本练习中，您将了解：

- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- 合并到代码库前，如何审查 Copilot 智能体模式生成的更改和测试。

## 场景

随着游戏列表不断增长，您希望允许用户按类别和发行商筛选。上一练习中已添加发行商辅助函数，现在将使用 Copilot 智能体模式完成数据层、UI 和测试的其余工作。

## 运行 Tailspin Toys 网站

进行更改前，先查看 Tailspin Toys 网站，了解它当前的功能。

该网站是一个以开发者为主题的棋盘游戏众筹平台。用户可以浏览游戏列表和详情。它是一个单体 Astro 应用，在构建时将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库——没有单独的后端 API，也没有客户端 UI 框架。可复用的数据访问辅助函数位于 `src/lib/`；交互功能使用范围较小的 Astro `<script>` 和标准 DOM API 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器。请按以下步骤在 GitHub Codespace 中运行：

1. 返回 codespace。上一练习后应该已切换回 `main`。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux），在 Codespace 中打开新终端。
3. 创建并切换到筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 运行以下命令启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器就绪后，您会看到类似以下 URL 的横幅：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果弹出对话框，询问是否打开 `http://localhost:4321` 的浏览器窗口，请选择 **x** 关闭。

5. 在终端中按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择 Astro 服务器地址 `http://localhost:4321`，打开网站。

> [!NOTE]
> 在 codespace 中选择终端里的 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 的专用通道，网站服务器现在由此提供服务！

### 浏览网站

网站运行后，可以查看其功能。主要包括：

- **主页**：显示棋盘游戏列表，包括标题、图片和说明。
- **游戏详情页**：选择游戏后会打开详情页，其中显示游戏标题、说明、发行商和类别等信息。

## 使用 Copilot 查看待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 启动新的对话。模式和模型的选择因练习而异；每个步骤会在适用时说明。

网站的初始实现已能正常运行，但我们希望添加更多功能。先查看待办事项。使用模板设置项目时，系统已自动创建待办议题——请 GitHub Copilot 列出这些议题。

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。**Agent** 智能体可以自主规划并跨多个文件实现更改、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 本研讨会作者并未偏好某个模型。编写本研讨会时使用了 Claude Sonnet 4.5，因此说明中也列出了该模型，希望帮助保持代码建议的一致性并提供良好体验。不过，LLM 的输出具有概率性，因此您收到的建议可能与说明不同。这完全正常。

> [!NOTE]
> 由于 LLM 的输出具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能完成任务。

3. 向 Copilot 发送以下提示，询问议题待办列表：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue**，运行列出所有议题的命令。
5. 查看生成的议题列表。

请注意，Copilot 还根据它认为对用户最有用的程度，对这些事项进行了优先级排序。

## 查看指令文件

让智能体生成代码前，先查看将用于提供上下文的指令文件。您将使用[用户界面 (UI)](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md) 指令文件，其中介绍了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 留意其中有关如何添加功能的总体指导，包括：
   - 架构概述。
   - 组件设计、可测试性和无障碍访问原则。
   - 指向各种文件类型专用指令文件的链接，包括 Astro、Tailwind CSS 和 Drizzle。

> [!TIP]
> 指令文件可以同时引用其他指令文件和项目文件。路径相对于指令文件的位置。这有助于复用、拆分复杂指令，以及提供示例和模板。

## 实现筛选功能

完成筛选功能至少需要对应用进行三项独立更新：

- 在数据层 (`src/lib/`) 添加或完善剩余筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页，加入筛选 UI。

此外，合并到代码库前必须运行并通过测试。Copilot 智能体模式可以帮您完成这些任务！现在开始添加功能。

1. 可以继续使用当前 Copilot 对话，也可以选择 **New Chat** 启动新对话。
2. 选择 **Add Context**、**Instructions** 和 **ui** 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 确认 Chat 视图的智能体下拉菜单中仍选择了 **Agent**。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5** 模型。
5. 使用以下提示，请 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目并找到相关文件。您应该会看到它查找数据层辅助函数、UI 和测试，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 会执行多个步骤，例如探索项目、修改文件和运行测试。根据任务复杂程度和代码库规模，可能需要几分钟。过程中编辑器可能会显示 **Keep** 和 **Undo** 按钮。Copilot 完成后，每项更改都会显示 **Keep** 或 **Undo**，因此工作进行时无需选择这些按钮。

7. Copilot 提示时，选择 **Continue** 运行测试。

   ![Copilot Chat 面板中询问是否运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这没有问题！Copilot 会在生成代码和测试之间反复迭代，直到完成任务且不再检测到错误。

   ![Copilot 智能体模式完整 Chat 会话截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码，查找潜在问题。

> [!CAUTION]
> 请记住，始终都要审查 Copilot 或其他 AI 工具生成的代码。

10. 返回运行网站的浏览器，试用新功能！
11. 确认功能正常且代码审查完毕后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结和后续步骤

恭喜！本练习中，您了解了如何使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加新功能。您学习了：

- GitHub Copilot 智能体模式如何跨 UI 和数据层实现新功能。
- Copilot 智能体模式如何探索项目、识别相关文件并协调更改。
- 合并到代码库前，如何审查 Copilot 智能体模式生成的更改和测试。

接下来，使用 [Playwright MCP 服务器测试功能][next-lesson]，并为其创建拉取请求。

### 额外探索练习 – 实现分页

随着游戏列表不断增长，需要启用分页。运用本练习学到的技能，请 Copilot 更新网站并实现分页。考虑以下事项：

- 遵循现有最佳实践，包括使用现有指令文件。
- 考虑分页的实现方式，例如允许用户选择每页条目数，还是使用固定值。
- 编写提示时，提供 Copilot 所需指导，确保它按您的要求实现功能。
- 可能需要与 GitHub Copilot 迭代，提出更改请求并提供上下文。这是使用 Copilot 的常规流程！

## 资源

- [Copilot 的 ask、edit 和 agent 模式：作用及适用场景][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
