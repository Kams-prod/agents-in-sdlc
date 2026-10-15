---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义指令][previous-lesson] |
|:--|

几乎所有组织都会面临技术债务，例如尚未解决的安全问题、需要更新的旧版代码，或因缺少时间而长期搁置的功能请求。GitHub Copilot 云智能体能够自主执行更新代码、添加功能等任务。智能体完成工作后，会创建一个草稿 PR，供开发人员审查。这样可以把繁琐任务交给智能体，加快开发过程，让开发人员专注于更重要的工作。

您将使用 Copilot 云智能体探索以下内容：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

## 场景

Tailspin Toys 希望解决一些技术债务。最初受雇创建网站的承包商没有做好文档——实际上，文档完全缺失。团队希望首先为应用程序中的所有导出函数添加 TSDoc 文档注释。

此外，设计团队准备改进游戏发现体验。他们希望每款游戏的详情页都显示相关游戏（即同类别的其他游戏），让访客可以继续浏览。目前不需要完善的实现，只需满足 UX 验收测试的基本需求。此项工作目前受到其他优先级更高的议题阻碍。

这些都是容易被搁置的任务，也很适合交给 Copilot 云智能体。云智能体可以异步处理这些任务，让开发人员专注于其他工作，之后再回来审查 Copilot 的成果，确认一切符合预期。

## 介绍 GitHub Copilot 云智能体

[GitHub Copilot 云智能体](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)可以像人类开发人员一样在后台执行任务。和与人类开发人员协作一样，可以通过多种方式分配工作，包括[将 GitHub 议题分配给 Copilot](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)。任务分配后，Copilot 会创建草稿拉取请求来跟踪进度、设置工作环境并开始处理任务。您可以在 Copilot 会话进行期间或完成后查看会话。建议方案准备好供您审查时，Copilot 会在拉取请求中标记您！

## 明确限定任务范围的重要性

GitHub Copilot 并没有什么魔法——使用它并不意味着可以跳过思考。即使看似简单的操作，深入分析后也可能很复杂，因此请留意[如何为 Copilot 云智能体限定任务范围][cloud-agent-best-practices]。将它视为 AI 结对程序员：分阶段开展工作，在实践中学习，并根据情况调整。生成式 AI 不会改变软件开发的基本原则。

## 此仓库中的自定义指令

前面的练习介绍了自定义指令及其引导 Copilot 的方式。如果您已经完成自定义指令练习，就见过 `.github/copilot-instructions.md` 的效果；否则，现在可以快速查看。将工作分配给 Copilot 云智能体前，请只读查看仓库中已有的指令文件，以便之后观察这些指令带来的影响。

在仓库的 GitHub Web UI 中，或在已启动的 codespace 中，打开以下文件：

- `.github/copilot-instructions.md` - 查看 **Code standards** 部分，尤其注意 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md` - 注意 `applyTo` frontmatter，它将指令限定在 `**/*.test.ts` 文件。

下一部分将 `Code lacks documentation` 议题分配给云智能体时，请留意由这些指令文件带来的 TSDoc 文档注释、TypeScript 约定和文件注释标题；稍后练习审查 PR 时还会再次提到它们。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要指定环境并运行设置脚本，以确保一切就绪。将任务交给 Copilot 时也一样，因为它会像软件工程师 (SWE) 一样执行工作。

云智能体使用 [GitHub Actions][github-actions] 作为工作环境。您可以通过创建[专用设置工作流][setup-workflow]来自定义环境，将其配置在 `.github/workflows/copilot-setup-steps.yml` 文件中，并在智能体开始工作前运行。这可以让智能体访问必需的开发工具和依赖项。实验开始前已预先配置该工作流，以便顺利完成练习。它会确保 Copilot 能够使用 Node.js、项目依赖项、端到端测试所需的浏览器二进制文件，以及为单个 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可在仓库的“Actions”选项卡中测试设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 设置为完成这些步骤所需的最低权限。Copilot 会使用独立令牌执行自己的操作。
    permissions:
      # 如果要在设置步骤中克隆仓库（例如安装依赖项），需要 contents: read 权限。如果设置步骤不克隆仓库，步骤完成后 Copilot 会自动执行克隆。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端 / 应用设置 - Node.js（整个应用现已使用 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，以便构建和测试时使用。
      - name: Set up the database
        run: npm run db:setup
```

此文件与其他 GitHub 工作流文件相似，但有几个关键点：

- 它只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理拉取请求之前，GitHub Actions 会运行此作业。
- 注意 `workflow_dispatch` 触发器，它允许您从仓库的 **Actions** 选项卡手动运行工作流。您可以借此测试工作流是否能成功运行，而不必等待 Copilot 启动它。

## 添加文档

每个人都知道文档的重要性，但大多数项目的文档都已过时，或完全缺失。这类技术债务通常无人处理，会降低工作效率、增加维护代码库的难度，也让新开发人员更难加入团队。幸运的是，Copilot 擅长生成文档，因此非常适合把这类任务交给 Copilot 云智能体。它会在后台生成所需文档，您将在后续练习中审查结果。

1. 在新的浏览器选项卡中访问 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 创建新议题。
5. 将 **Title** 设置为 `Code lacks documentation`。
6. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

  ![Copilot 分配详情](../../../_images/ex4-assign-copilot-details.png)

10. 选择 **Pull Requests** 选项卡。
11. 打开新生成的拉取请求 (PR)，标题类似 `[WIP]: Code lacks documentation`。如果列表中还没有新 PR，请稍等片刻并刷新浏览器窗口。
12. 几分钟后，您应该会看到 Copilot 创建了待办事项列表。

> [!NOTE]
> Copilot 生成的待办事项列表可能需要几分钟才会出现在 PR 中。Copilot 正在创建工作环境（运行前面提到的工作流）、分析项目并确定解决问题的最佳方法。

13. 审查待办事项列表和计划执行的任务。
14. 向下滚动拉取请求时间线，您应该会看到 Copilot 已开始处理议题。
15. 选择 **View session** 按钮。

  ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 可能需要刷新窗口才能看到更新后的指示器。

16. 您可以滚动查看实时会话，观察 Copilot 如何解决问题，包括探索代码、了解当前状态、暂停思考并确定合适的计划，以及创建代码。

这可能需要几分钟。Copilot 云智能体的主要目标之一是异步执行任务，让我们腾出时间处理其他工作。现在就利用这一特性：再分配一个任务给 Copilot 云智能体，然后转而为应用编写新功能代码。

## 在游戏详情页添加相关游戏版块

如前所述，GitHub Copilot 云智能体的一大优势是可以并行分配工作，让您与智能体分别专注于不同任务。即使为设计团队添加相关游戏版块不一定需要很长时间，也可以把时间用于其他工作。现在将该任务交给 Copilot 云智能体！

1. 返回 github.com 上的仓库。
2. 选择 **Issues** 选项卡。
3. 选择 **New issue** 打开新议题对话框。
4. 选择 **Blank issue** 使用空白模板。
5. 将 **Title** 设置为 `Show related games on the game details page`。
6. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. 选择 **Create** 创建议题。
8. 在右侧选择 **Assign to Copilot**，打开分配对话框。

  ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

9. 选择 **Assign**。

很快，您应该会在议题的第一条评论中看到 👀，表示 Copilot 已开始处理！

![Copilot 使用眼睛表情符号表示正在处理议题](../../../_images/ex4-issue-eyes-emoji.png)

Copilot 现在正认真处理您的第二项请求！Copilot 云智能体的工作方式与软件工程师类似，因此无需主动监控；完成后再审查即可。现在来了解如何创建和使用自定义智能体。

## 总结和后续步骤

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。您可以将议题分配给云智能体，让它异步处理任务。可以使用 Copilot 解决技术债务、创建新功能，或协助将代码从一个框架迁移到另一个框架。

您探索了以下概念：

- 自定义代码生成环境。
- 确保安全地执行操作。
- 明确限定议题范围的重要性。
- 将议题分配给 Copilot。

云智能体正在后台处理任务，现在可以转而创建和使用自定义智能体。[Copilot 云智能体也支持使用 MCP 服务器][cloud-agent-mcp]，并可以使用此前模块介绍过的自定义指令。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

| [下一课：自定义智能体 →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
