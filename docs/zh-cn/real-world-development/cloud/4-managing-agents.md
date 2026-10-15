---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

在前几项练习中，您要求 Copilot 云智能体完成了三个独立任务，以改善用户体验并添加功能。云智能体虽然可以异步自主工作，但监控这些任务仍然很重要。

GitHub 提供了多种工具来管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。您可以在这个任务控制中心查看所有拥有未关闭拉取请求 (PR) 的智能体任务、查看其执行的操作，甚至引导正在进行的会话。

在本课中，您将：

- 探索智能体页面，监控云智能体任务。
- 引导正在进行的会话，请求添加更多功能。

## 场景

为智能体分配高对比度模式任务后，团队发现同时添加浅色模式也很合适。既然网站样式和切换功能已经在更新，将这项功能一并加入很合理。您将引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的所有任务当前状态。

1. 访问 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务列表。您应能看到分配给 Copilot 的任务，包括：
    - 更新代码库文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一项正在运行的任务，查看 Copilot 执行过的操作。可能包括：
    - 从仓库检出代码。
    - 创建 Copilot 工作环境。
    - 设置 MCP 服务器。
    - 执行完成指定任务所需的各种步骤。

> [!NOTE]
> 具体列出的步骤会因 Copilot 的工作状态和处理方式而异。

4. 同时留意右侧显示的拉取请求 (PR) 面板。您可以在此查看 PR 和已更改文件，以便进一步监控。

## 引导云智能体

既然已查看正在进行的任务，现在请 Copilot 在实现高对比度模式时同时添加浅色模式切换选项。

1. 选择与添加高对比度模式相关的会话。具体标题会因 Copilot 使用的名称和当前工作状态而异。

    ![任务控制中心中的无障碍访问会话](../../../_images/ex6-accessibility-session.png)

2. 观察几分钟，直到会话显示设置已完成并开始实际工作。看到类似下方的消息时，即表示已开始工作。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中云智能体任务截图，其中突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 请注意 Copilot 如何确认收到提示，并将其纳入工作流程。

## 让 Copilot 完成工作

与之前一样，Copilot 会开始处理更新后的任务！它会先完成收到消息时正在执行的步骤，然后将新请求纳入工作流程。

和之前一样，这需要几分钟。此时正好可以暂停一下，回顾迄今学到和探索的内容。

## 总结和后续步骤

本课介绍了 Copilot 智能体页面，这是监控和引导 GitHub Copilot 云智能体任务的中心枢纽。您可以在任务控制中心跟踪所有正在进行和已完成的任务、审查执行中的工作，并重定向任务以调整范围或提供更多指导。

您探索了以下概念：

- 使用任务控制中心和智能体页面监控云智能体任务。
- 重定向正在进行的会话，请求添加更多功能。

Copilot 正在完成无障碍访问功能。接下来，您将[迭代改进 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代改进 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
