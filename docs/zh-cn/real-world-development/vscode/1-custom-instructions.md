---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取相应上下文。您可以通过多种方式向 Copilot 提供特定上下文，其中重要的一种是[指令文件][instruction-files]，它们用于指导代码生成。

在本练习中，您将：

- 了解项目专属上下文、编码指南和文档标准如何通过仓库自定义指令和路径范围指令文件传递给 Copilot；
- 使用当前指令生成筛选功能的第一个数据切片（发行商辅助函数）；
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准；
- 重新运行相同提示，观察生成代码如何遵循新标准；
- 将指令更新和筛选功能切片提交到 `main`，供下一练习中的 Copilot 使用更新后的指导。

> [!CAUTION]
> 生成的代码可能不完全符合您设定的标准，因为 Copilot 的输出具有概率性。本练习的重点是观察更新指令后行为的*趋势变化*，而不是让生成结果与示例逐字一致。

## 指令文件

### 场景

Tailspin Toys 制定了一系列开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代感。
- 代码应通过 TSDoc 文档注释添加说明。
- 每个文件开头都应有一个注释块，说明该文件的用途。

使用指令文件可确保 Copilot 获取相关信息，并依照上述实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，使其更好地理解您的编码风格和要求。您可以据此引导 Copilot 提供更相关的建议和代码片段，也可以指定首选编码约定、库以及注释习惯。指令既可以适用于整个仓库，也可以针对特定任务或文件类型。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给 Copilot 的单个仓库指令文件，适用于仓库中的**每个**请求。它应包含适用于大多数 Copilot 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型提供指导。例如，您可以为 TypeScript 或 Astro 等语言，以及创建 UI 组件或新单元测试等任务提供指引。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一处编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用仓库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot [还支持使用其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本研讨会的范围。不过，示例项目展示了一种有代表性的方法。大致而言：

- 将 `copilot-instructions.md` 中的指令聚焦在项目级指导，例如项目内容、结构和全局编码标准。
- 使用 `*.instructions.md` 文件为特定文件类型（如单元测试、Astro 组件和数据层）或任务提供专门指令。
- 使用自然语言，保持指导清晰，并提供代码正确和错误写法的示例。

创建指令文件并没有唯一的方式，使用 AI 也是如此。通过实践，您会找到适合项目的方法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应拥有完善的指令文件集合。查看此项目中的指令文件时，可能会发现其中包含多种任务的指引，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同使用界面的操作各不相同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。相关课程会在适用时介绍这些操作。
>
> 想要寻找模板或起点？可以查看 [awesome-copilot][awesome-copilot]，其中包含大量指令文件、自定义智能体和其他资源。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 探索此项目中的自定义指令文件

项目模板已包含一组指令文件。先查看已有内容，再进行更改，观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 浏览文件，留意项目简介和编码指导。这些指令适用于您与 Copilot 的所有交互。
4. 打开 `.github/instructions` 文件夹并查看其中内容。注意其中包含 Astro 文件、Drizzle 数据层、测试等指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它定义了指令适用文件的 glob 模式。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和现有项目文件的链接。这种组织方式可将复杂指令拆分为较小的可复用文件，并为 Copilot 提供参考示例。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目编码标准，但尚未要求在代码中添加文档注释。接下来将添加 TSDoc 文档注释和文件注释标题规则。

## 为更改创建分支

按照最佳实践，为更改创建分支。

1. 返回上一个练习中的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace，打开 Copilot Chat 面板，然后选择 **New Chat** 启动新的对话。模式和模型的选择因练习而异；每个步骤会在适用时说明。

为了观察自定义指令的影响，先在当前指令文件下发送提示。稍后更新指令后，再次发送相同提示并比较结果。

1. 关闭之前练习中打开的编辑器选项卡，确保 Copilot 只获取所需上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道辅助函数应该放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，这样 Copilot 才能应用文件更改。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改辅助函数文件和测试。
6. 注意，生成的辅助函数是类型化函数，以 `db` 客户端作为第一个参数，并返回类型化的发行商数组。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成的代码**缺少** TSDoc 文档注释和文件级注释标题。

> [!CAUTION]
> Copilot 的输出具有概率性，因此它可能会在没有明确要求的情况下添加文档注释。即使如此也没关系；本练习重点是观察更新指令后行为的一致性变化。

## 添加新的仓库标准

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录仓库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，大约位于第 27 行。注意，该部分记录项目编码标准，但尚未要求在代码中添加文档注释，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，指令文件可以放在仓库级别（`.github/copilot-instructions.md`），以提供全局指导；也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指引。仓库级文件适合存放刚添加的文档注释规则等项目级标准。

## 重新运行提示并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦于正确文件。
3. 发送与之前**相同的提示**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 注意，生成的文件现在会以类似以下注释块开头：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 注意，生成的函数现在包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

您刚刚引导 Copilot 遵循一项新项目标准，并将其应用于后续练习将继续使用的真实代码。

## 提交指令并推送分支

指令文件和其他资源一样，都应使用同一套源代码管理流程。现在提交并推送分支。

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 在终端中运行以下命令，确认指令和辅助函数更改已存在：

   ```bash
   git status
   ```

3. 在终端中暂存并提交指令更新，以及 Copilot 为辅助函数基础所更改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示了其他配套更新（例如测试），也请添加这些文件。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 在终端中将分支推送到仓库：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送完成后，需要创建拉取请求，并关联待办事项中的文档标准议题。您可以手动完成，也可以让已连接到项目的 GitHub 工具协助 Copilot。请提示 Copilot 查找议题、创建关联该议题的 PR，然后合并。

> [!NOTE]
> 这是第一个让 Copilot 代表您操作 GitHub 的练习。项目模板已为工作区配置 GitHub 工具，因此无需额外设置。Copilot 首次使用工具时，VS Code 可能会提示您登录 GitHub。按提示授权即可。后续课程将介绍这种连接的工作原理（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确保智能体选择器中已选择 **Agent**。
3. 请 Copilot 查找与更新指令文件相关的议题，并从当前分支创建 PR，说明指令更新和新增的发行商辅助函数基础：

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会开始查找议题并创建 PR。
5. Copilot 请求代表您执行 GitHub 操作时，请审查命令，并酌情选择 **Allow**。
6. PR 创建后，请 Copilot 将 PR 合并到 `main`，并将本地分支切回 `main`：

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot 请求代表您执行 GitHub 操作和 shell 命令时，请审查命令，并酌情选择 **Allow**。

您已借助 GitHub Copilot 创建并合并了拉取请求！

## 总结和后续步骤

您了解了 Copilot 如何从指令文件获取上下文，并使用 VS Code 中的 Copilot Chat：

- 发送代码生成提示，观察现有指令的影响；
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准；
- 重新运行相同提示，观察生成代码如何遵循新标准；
- 将指令更新和辅助函数基础提交到 `main`，供下一练习继续使用。

接下来，让 Copilot 在[智能体模式][next-lesson]下应用这些指令，跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://awesome-copilot.github.com/
