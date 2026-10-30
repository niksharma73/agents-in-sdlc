---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前两项练习中，你让 Copilot 云智能体处理了三项不同任务，用于改进用户体验和添加功能。云智能体虽然能够自主异步工作，但监控这些任务仍然很重要。

你可以使用多种工具管理分配给云智能体的任务，包括 GitHub.com 上的[智能体页面][agents-page]。在这个任务控制中心，你可以查看所有拥有未关闭拉取请求（PR）的智能体任务，查看它们执行的操作，甚至引导正在进行的会话。

本课将介绍如何：

- 浏览智能体页面并监控云智能体任务。
- 引导正在进行的会话，要求添加其他功能。

## 场景

将高对比度模式任务分配给智能体后，团队意识到也可以趁此机会添加浅色模式。既然正在更新网站样式并添加切换功能，加入这项功能很合理。现在，你要引导智能体，确保它同时添加浅色模式和高对比度模式。

## 查看 Copilot 云智能体任务

查看当前分配给 Copilot 云智能体的所有任务的状态。

1. 转到 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主窗格和左侧窗格中的任务列表。你应该能看到已分配给 Copilot 的任务，包括：
    - 更新代码文档。
    - 生成用于修改游戏的 API。
    - 为网站添加高对比度模式。
3. 选择一个正在运行的任务，查看 Copilot 已执行的操作，例如：
    - 从存储库检出代码。
    - 为 Copilot 创建工作环境。
    - 设置 MCP 服务器。
    - 执行完成任务所需的各种步骤。

> [!NOTE]
> 列出的具体步骤会根据 Copilot 的工作状态和执行方案而变化。

4. 留意右侧出现的 PR 窗格。在此可以查看 PR 和更改的文件，以便进一步监控任务。

## 引导云智能体

现在你已了解如何查看正在执行的任务，接下来让我们在智能体处理高对比度模式时，要求它同时添加浅色模式切换开关。

1. 选择与添加高对比度模式相关的会话。具体标题取决于 Copilot 使用的名称和当前工作状态。

    ![任务控制中心中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 观察会话几分钟，直到会话显示设置已完成并开始执行任务。此时，你会看到类似下面的消息。
3. 在 **Steer active session while Copilot is working** 对话框中，输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中的云智能体任务，突出显示了 Steer active session while Copilot is working 对话框](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 留意 Copilot 如何确认收到提示并将其纳入工作流程。

## 让 Copilot 继续工作

与之前一样，Copilot 会继续处理更新后的任务！在你发送消息时，它会先完成当前步骤，然后将新要求纳入工作流程。

这同样需要几分钟。你可以利用这段时间暂停并回顾迄今为止学到和体验的内容。

## 总结与后续步骤

本课介绍了 Copilot 智能体页面，它是监控和引导 GitHub Copilot 云智能体任务的中心位置。在这个任务控制中心，你可以跟踪所有进行中和已完成的任务、审查智能体正在执行的工作，甚至重新引导正在进行的任务以调整范围或补充指导。

本课介绍了以下概念：

- 浏览任务控制中心和智能体页面，监控云智能体任务。
- 重新引导正在进行的会话，要求添加其他功能。

Copilot 正在处理无障碍功能。接下来，我们将了解如何[迭代优化 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代优化 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
