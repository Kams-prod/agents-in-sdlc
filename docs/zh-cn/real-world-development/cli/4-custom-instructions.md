---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "了解仓库指令，添加文档标准，并将其应用于筛选代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务需要以特定方式完成，就应让 Copilot 能够获取相应指导。[指令文件][instruction-files]不仅说明您想要什么代码，还描述代码应如何组织。完成筛选功能后，您将了解 Copilot 使用的指令、添加文档标准，并将其应用于代码。

在本课中，您将：

- 了解仓库指令和路径范围指令文件如何传递给智能体。
- 更新指令文件，确保遵循编码标准。
- 观察指令文件对代码的影响。

## 场景

Tailspin Toys 制定了一系列开发实践指南和要求，包括：

- 注释应说明意图和不明显的设计决定，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应使用 TSDoc/JSDoc 说明用途、参数和返回值；如果存在可注入的 `db` 参数，也应说明。
- 可复用 Astro 组件应说明其 `Props` 契约；相关代码更改时，注释也应保持准确。
- 应保留现有格式和 lint 指引。

通过指令文件，确保 Copilot 了解这些实践要求，并据此完成任务。

## 指令文件

自定义指令可以向 Copilot 提供上下文和偏好，使其更好地理解您的编码风格和要求。您可以借此引导 Copilot 提供更相关的建议和代码片段，也可以指定首选编码约定、库以及注释习惯。指令既可以适用于整个仓库，也可以针对特定任务或文件类型。

指令文件有两种：

- `.github/copilot-instructions.md` 是发送给 Copilot 的单个仓库指令文件，适用于仓库中的**每个**请求。它应包含适用于大多数聊天或 CLI 请求的项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md` 文件可针对特定任务、语言、文件类型或其他需求提供指导。

> [!NOTE]
> 其他指令格式及其支持情况因环境而异。使用特定格式前，请参阅[自定义指令支持参考][custom-instructions-support]。

## 查看此项目的自定义指令文件

入门项目中已包含一组指令文件。先查看已有内容，再进行更改，观察其影响。

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。此仓库中的每次 Copilot 交互都会使用这些指令。
4. 打开 `.github/instructions` 文件夹并查看其中的文件。注意其中包含 Astro 文件、Drizzle 数据层、测试等指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`。注意顶部的 `applyTo` 字段，它定义了指令适用文件的 glob 模式。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看其中指向其他指令文件和项目文件的链接。这样可以将复杂指令拆分成较小的可复用文件，并提供供 Copilot 参考的示例。

## 更新指令以符合团队要求

现有文件是不错的起点，但还缺少一项要求。修改核心 `copilot-instructions.md` 文件，确保新生成的 TypeScript 代码包含 TSDoc 注释。

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，大约位于第 35 行。
2. 在最后一条后添加以下内容：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存！

## 使用更新后的指引

Copilot CLI 会在对话开始时加载仓库指令。保存更新后的指令后，退出并恢复筛选对话，以便 Copilot 在保留功能上下文的同时加载新指引。

1. 如果筛选对话仍处于打开状态，请输入 `/exit` 关闭。Copilot CLI 会自动保存对话。
2. 在 `game-filters-cli` 分支的仓库根目录中恢复已保存的对话：

   ```bash
   copilot --resume
   ```

   如果出现提示，请选择筛选对话，并确认当前检出的是 `game-filters-cli` 分支。
3. 请 Copilot 应用更新后的指引：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并查看已更改的 TypeScript 文件。注意新生成的 TSDoc 注释，并确认其准确说明代码。

## 总结和后续步骤

您了解了 Copilot 如何从指令文件获取上下文，并将新标准应用于功能。具体来说，您：

- 了解了仓库指令和路径范围指令文件如何传递给智能体。
- 更新了指令文件，确保遵循编码标准。
- 观察了指令文件对代码的影响。

接下来，您将[自定义并运行可复用的 quality-checks 技能][next-lesson]，以一致地运行 lint 和测试。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更优 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
