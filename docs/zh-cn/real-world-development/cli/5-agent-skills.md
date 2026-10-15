---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "探索现有的 quality-checks 技能，自定义其报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码并不只是编写代码。我们已经手动验证了代码能否正常运行，也使用指令文件确保代码符合标准。但测试、lint 和其他持续集成 (CI) 工作该如何处理？

这类任务很适合使用**智能体技能**！技能可以帮助 Copilot 了解如何正确执行这类操作。

在本课中，您将：

- 探索现有的 `quality-checks` 技能。
- 自定义技能结果的格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告格式，让结果更易于阅读。

## 指令、脚本和资源

智能体技能会打包可复用的任务指令、可执行脚本和配套资源，供智能体按需加载。技能本质上是一个文件夹，其中包含名为 `SKILL.md` 的 Markdown 文件。该文件包含用于定义技能名称和说明的 frontmatter、技能功能概述，以及关于何时调用技能的指导。技能文件夹还可以包含供技能使用的脚本和其他资源子目录。

> [!NOTE]
> 技能不一定需要包含其他文件或文件夹。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目现有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为可供团队共享和复用的仓库资源；也可以放在用户技能目录 `~/.copilot/skills` 中。

## 探索技能

接下来查看 Tailspin Toys 团队为运行单元测试、lint 和类型检查创建的 `quality-checks` 技能。

1. 返回 Codespace。在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。description 会帮助 Copilot 判断何时调用技能。
3. 阅读指令，注意它们如何引导 Copilot 执行测试和 lint。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

Copilot CLI 可以直接调用技能，也可以通过自然语言调用。现在请 Copilot 运行该技能中的三项检查。

1. 返回处于 Interactive 模式的筛选对话。
2. 使用以下提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 留意最后生成的报告。

## 自定义报告

我们希望报告更清晰，说明运行了哪些命令、是否成功，以及工具实际报告的内容。现在更新技能，让它生成所需报告。

1. 返回 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改已完成，现在试用新格式！Copilot CLI 可以在不重启对话的情况下重新加载已编辑的技能。

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前完全相同的提示：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 查看最后生成的报告，并与第一次报告比较。

## 总结和后续步骤

您已自定义并使用现有智能体技能。在本课中，您：

- 探索了用于运行单元测试、lint 和类型检查的 `quality-checks` 技能。
- 自定义了技能结果的格式。
- 重新加载并运行了该技能。

这项更改会与筛选功能一起纳入功能 PR。接下来，使用 [Playwright MCP 服务器][next-lesson]让 Copilot 直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，并非额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和打包的导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
