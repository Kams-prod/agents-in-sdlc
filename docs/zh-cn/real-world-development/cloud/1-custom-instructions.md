---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，或者 Copilot 需要了解背景信息，就要确保它能获取相应上下文。[指令文件][instruction-files]可提供这些指导，让 Copilot 不仅了解您想让它做什么，也了解应该如何完成。

在本练习中，您将：

- 了解项目专属上下文、编码指南和文档标准如何通过仓库自定义指令和路径范围指令文件传递给 Copilot；
- 在 `.github/copilot-instructions.md` 中添加新的仓库级标准。

> [!NOTE]
> 与 VS Code 和 CLI 开发环境不同，本练习不会使用“前后对比”提示——Copilot 云智能体会在 GitHub 议题上异步工作，因此难以实时并排展示影响。稍后审查云智能体生成的拉取请求时，您会看到指令文件带来的影响。

## 指令文件

### 场景

Tailspin Toys 制定了一些开发实践指南和要求，包括：

- 数据层始终需要单元测试。
- UI 应使用深色模式并具有现代感。
- 代码应通过 TSDoc 文档注释添加说明。
- 每个文件开头都应有一个注释块，说明该文件的用途。

使用指令文件可确保 Copilot 获取相关信息，并依照上述实践完成任务。

### 自定义指令

自定义指令可以向 Copilot 提供上下文和偏好，使其更好地理解您的编码风格和要求。您可以据此引导 Copilot 提供更相关的建议和代码片段，也可以指定首选编码约定、库以及注释习惯。指令既可以适用于整个仓库，也可以针对特定任务或文件类型。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给 Copilot 的单个仓库指令文件，适用于仓库中的**每个**请求。它应包含适用于大多数 Copilot 聊天或 CLI 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务或文件类型提供指导。例如，您可以为 TypeScript 或 Astro 等语言，以及创建 UI 组件或新单元测试等任务提供指引。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 的代码生成，不用于代码补全或下一处编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体在生成代码时都会使用仓库级指令文件和带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> 此外，Copilot [还支持使用其他标准的指令文件][custom-instructions-support]，包括 AGENTS.md 和 CLAUDE.md。

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

花点时间阅读此仓库随附的指令文件——包括一个核心 `copilot-instructions.md` 和适用于各种任务的一组 `*.instructions.md` 文件。您可以在编辑器或 GitHub Web UI 中打开这些文件。

1. 打开 `.github/copilot-instructions.md`。
2. 浏览文件，留意项目简介以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还包含嵌套的 **GitHub Actions Workflows** 指南。这些指令适用于您与 Copilot 的任何交互。
3. 打开 `.github/instructions` 文件夹并查看其中内容。注意其中包含 Astro 文件、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它设置了一个相对于仓库根目录的 glob 模式，用来确定指令适用的文件。例如，任何符合 `**/*.test.ts` 的 TypeScript 测试文件都会匹配。
5. 留意此项目针对单元测试创建的具体指令。
6. 最后，打开 `.github/instructions/drizzle.instructions.md` 并滚动到末尾。注意其中指向其他指令文件（例如 `unit-tests.instructions.md`）和项目现有文件的链接。这样可以将较大的指令集拆分为较小的可复用文件，并为 Copilot 提供可参考的示例。（这些路径相对于指令文件，而不是仓库根目录。）

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 部分记录了项目的编码标准，但尚未要求在代码中添加文档注释。接下来的步骤会添加 TSDoc 文档注释和文件注释标题规则。

## 添加新的仓库标准

接下来是本练习中唯一需要编辑的部分：添加一条项目级规则，要求使用 TSDoc 文档注释和文件级注释标题在代码中编写文档。后续在本开发环境中分配议题时，云智能体会使用此规则。

编辑前，先创建一个分支（云智能体会从议题目标分支读取指令，但您仍应整洁地提交编辑）：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

如前所述，`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在记录仓库编码标准，以改进代码建议。

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，应位于第 27 行附近。注意，该部分记录项目编码标准，但尚未要求在代码中添加文档注释，这也是生成的辅助函数没有文档注释的原因。
3. 在现有标准下方添加以下 Markdown 内容，要求 Copilot 添加文件注释标题和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 如上一课所述，指令文件可以放在仓库级别（`.github/copilot-instructions.md`），以提供全局指导；也可以使用 `*.instructions.md` 为特定语言、文件类型或任务提供指引。仓库级文件适合存放刚添加的文档注释规则等项目级标准。

## 提交、推送并合并指令更新

云智能体会读取议题目标分支中的指令。在下一练习中将议题分配给 Copilot 时，Copilot 会从 `main` 分支创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用它们。

1. 暂存并提交：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 到 `main` 的拉取请求并合并。随后，云智能体处理下一练习中分配的议题时就会读取这些指令。

> [!TIP]
> 如果希望在本研讨会中直接使用 `main`，可以跳过分支步骤，直接提交到 `main`。此处创建分支是为了模拟真实项目中的工作方式。

## 总结和后续步骤

您了解了 Copilot 如何从指令文件获取上下文，并添加了新的仓库级标准。之后，在本开发环境中审查云智能体生成的拉取请求时，就能看到该标准的实际效果。

接下来，将[第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
