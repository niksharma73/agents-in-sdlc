---
title: "练习 5：迭代优化 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

## 审查工作成果

在本实验中，你与 GitHub Copilot 协作完成了多项任务，以改进用户体验并添加功能。你要求 Copilot 为代码添加文档、构建供设计团队迭代的相关游戏功能，以及实现包括高对比度和浅色模式切换开关在内的无障碍功能。接下来，我们将检查代码更改；必要时，也可以向 Copilot 提供反馈以改进成果。

### 场景

如前所述，加入生成式 AI 并不会改变软件设计和 DevOps 的基本原则。我们始终需要审查生成的代码，并遵循常规 DevOps 流程。接下来，在为团队其他成员启用审查之前，我们先审查 GitHub Copilot 为文档、相关游戏功能和无障碍功能提出的更改。

## 安全性与 GitHub Copilot 云智能体

Copilot 云智能体会在无人监督的情况下异步执行任务，因此设置了一些安全限制，以确保操作安全。这些限制包括：

- Copilot 只有存储库的读取权限，并且仅对其创建代码所用的分支具有写入权限。
- 云智能体在 GitHub Actions 中运行，并在其中创建独立的临时工作环境。
- 任何 GitHub Actions 工作流在运行前都需要人工批准。
- [默认情况下，外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

我们先查看 GitHub Copilot 云智能体创建的第一个拉取请求（PR），也就是为代码添加文档的 PR。你将使用 GitHub.com 中的标准 PR 界面完成审查。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是**预期行为**，因为 Copilot 默认无法访问大多数外部资源，包括外部 MCP 服务器。如果需要，可以[自定义或禁用 Copilot 云智能体的防火墙][agent-firewall]。

1. 返回 github.com 上的存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 的拉取请求。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作。这可能需要几分钟，你可以稍作休息，或回顾迄今为止学到的内容。

4. PR 准备就绪后，选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加了 TSDoc 文档注释和其他文档的代码。具体更改可能会有所不同。

    查看更改时，留意 TSDoc 文档注释、TypeScript 规范和文件注释头。这些内容来自你在练习 2 开始时查看的自定义指令文件。

6. 审查更新并确认无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 拉取请求的检查区域应显示已排队的工作流。如果一切正常，针对这个单体 Astro 应用的项目检查就会通过。此过程可能需要几分钟。

## 请求 GitHub Copilot 进行更改

与 Copilot 协作并非单向过程。你可以像在拉取请求中提及团队其他成员一样，在 PR 评论或代码行内评论中提及 Copilot。Copilot 会读取这些评论，并启动另一个会话来处理请求。由于结果并非确定，因此我们无法提供固定的提示文本。你可以要求 Copilot 更新以下内容：

- 在每个代码文件开头添加注释头，简要说明文件的作用。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建 README，介绍 Astro 应用的结构。

1. 添加评论，请求修改生成的文档，并像提及其他用户一样提及 **@copilot**。你可以从上述建议中选择一项，也可以提出其他希望 Copilot 为代码库补充的文档内容。
2. 选择 **View Session**，观察 Copilot 如何执行任务。留意 Copilot 如何启动新会话来完成更新。
3. 选择 **Back to pull request**，返回拉取请求。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，拉取请求中会出现新的提交。
5. 选择 **Files changed** 选项卡审查更改。

你可以继续迭代，直到满意为止。确认满意后，可以将 PR 从草稿状态改为就绪状态，然后合并到 main 分支。

![将 PR 转为就绪状态](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

现在，返回 Copilot 为解决“在游戏详细信息页面显示相关游戏”议题而创建的 PR。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Show related games on the game details page** 的 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有等待批准的工作流提示。
7. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 拉取请求的检查区域应显示已排队的工作流。如果一切正常，针对这个单体 Astro 应用的项目检查就会通过。此过程可能需要几分钟。
9. **可选：**你也可以在 Codespace 中切换到该分支，手动测试相关游戏功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称，例如 **copilot/fix-8**）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已经构建了相关游戏功能！与之前一样，你可以通过迭代请求 Copilot 更新。例如，可以要求 Copilot 调整显示的相关游戏数量，或添加文件注释头和 TSDoc 文档注释（请记住，分配此任务时，你还没有更新自定义指令！）。与之前一样，可以在 **Conversation** 选项卡中添加评论，Copilot 会读取评论并启动新会话。

## 审查无障碍功能

最后，审查通过无障碍自定义智能体实现的功能。这个 PR 应同时包含你在练习 3 中要求实现的高对比度模式，以及在练习 4 的任务控制中心中提出的浅色模式。

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 选择标题类似 **Add high contrast mode to website** 的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，拉取请求会带有 **[WIP]** 标记。此时请等待 Copilot 完成工作，这可能需要几分钟。

4. 选择 **Files changed** 选项卡审查生成的代码。
5. 审查实现，尤其留意：
   - 用于切换模式的 UI 组件。
   - 用于保存用户偏好的本地存储。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - 无障碍属性（ARIA 标签、键盘导航等）。
   - 管理模式切换的 JavaScript/TypeScript 代码。

6. 确认更新无误后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有等待批准的工作流提示。
8. 如果有等待批准的工作流，请选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 拉取请求的检查区域应显示已排队的工作流。如果一切正常，针对这个单体 Astro 应用的项目检查就会通过。此过程可能需要几分钟。
10. **可选：**你可以在 Codespace 中切换到该分支，手动测试无障碍功能。转到 Codespace，打开终端并运行以下命令（将 `<branch-name>` 替换为 Copilot 创建的分支名称）：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    然后启动应用，并在浏览器中测试高对比度和浅色模式切换开关，确认功能正常且页面重新加载后仍能保留设置。

留意无障碍自定义智能体如何引导 Copilot 遵循无障碍最佳实践来实现这些功能。如果发现无障碍方面的问题或改进空间，可以像处理之前的 PR 一样，在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派任务

云智能体最适合处理真实的待办事项，让你能专注于其他工作。你可以在自己的存储库中创建更多议题并分配给 Copilot，以培养这种工作习惯。以下是一些想法：

- 在游戏详细信息页面添加支持者兴趣表单。
- 为游戏列表页实现分页。
- 为数据访问辅助函数添加输入验证和错误处理。

## 总结

你已完成云智能体环境中的全部练习。在这些课程中，你：

- **检查了此存储库自带的自定义指令文件**，以便之后了解它们如何影响云智能体的输出。
- **将议题分配给 Copilot 云智能体**，并观察它如何设置环境、制定计划并异步执行任务。
- **创建并使用自定义智能体**实现无障碍功能，添加高对比度和浅色模式切换开关。
- **将 Copilot 智能体页面用作任务控制中心**，监控并引导正在进行的无障碍会话。
- **审查并迭代优化云智能体创建的拉取请求**，通过提及 `@copilot` 请求更改并批准工作流。

## 审查与后续步骤

你已完成云智能体环境中的练习。如果想继续探索，其他环境可以补充本次实践的内容：

- 🖥️ **[VS Code 环境](../../vscode/)** — 在 IDE 中直接探索 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 环境](../../cli/)** — 在终端中交付两项经过审查的更改，并使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 桥接功能将工作交回云智能体。

在自己的存储库中，可以尝试以下后续任务：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为不同技术栈设置 `copilot-setup-steps.yml`。

## 资源

- [GitHub Copilot][github-copilot]
- [关于 Copilot 智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]
- [配置 Copilot 云智能体防火墙][agent-firewall]

---

| [← 上一课：管理智能体][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
