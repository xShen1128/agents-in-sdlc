---
title: "练习 4 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前几项练习中，你让 Copilot 云智能体执行了三项改进用户体验和添加功能的任务。云智能体虽然可以异步自主工作，但仍需监控这些任务。

GitHub.com 上的[智能体页面][agents-page]等工具可用于管理分配给云智能体的任务。你可以在这个 mission control 中查看所有有未关闭 PR 的智能体任务、查看它们执行的操作，甚至在会话进行期间提供引导。

本课将介绍：

- 如何使用智能体页面监控云智能体任务。
- 如何引导正在进行的会话，为它添加功能要求。

## 场景

团队已让智能体创建高对比度模式，随后又决定添加浅色模式。由于智能体已经在更新网站样式并添加切换功能，因此希望引导它一并实现浅色模式。

## 查看 Copilot 云智能体任务

查看分配给 Copilot 云智能体的任务状态：

1. 打开 [Copilot 智能体页面](https://github.com/copilot/agents)。
2. 查看主面板和左侧面板中的任务，包括为代码库添加文档、生成修改游戏的 API，以及为网站添加高对比度模式。
3. 选择一个正在运行的任务，检查 Copilot 已执行的操作，例如从存储库检出代码、创建工作环境、设置 MCP 服务器和执行任务步骤。

> [!NOTE]
> 具体显示的步骤会因 Copilot 的工作进度和采用的方法而异。

4. 查看右侧显示的 PR 面板，其中包含 PR 和已更改文件，便于进一步监控。

## 引导云智能体

现在要在高对比度模式任务中要求 Copilot 同时添加浅色模式：

1. 选择与高对比度模式相关的会话。会话的确切标题取决于 Copilot 使用的名称和当前工作状态。

    ![Mission control 中的无障碍会话](../../../_images/ex6-accessibility-session.png)

2. 等待几分钟，直到会话完成设置并开始执行任务。
3. 在 **Steer active session while Copilot is working** 对话框中输入以下提示：

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![智能体页面中突出显示 Steer active session while Copilot is working 对话框的云智能体任务截图](../../../_images/ex6-steer-cloud-agent-task.png)

4. 按 <kbd>Enter</kbd> 发送提示。
5. 查看 Copilot 如何确认收到提示并将其纳入工作流程。

## 等待 Copilot 完成工作

Copilot 会先完成发送提示时正在执行的步骤，然后将新要求纳入任务。等待期间，可以回顾目前学到的内容。

## 总结与下一步

本课介绍了 Copilot 智能体页面，它是监控和引导云智能体任务的中心。你可以跟踪正在进行和已完成的任务、查看执行过程，或调整进行中任务的范围并补充指导。

你已使用 mission control 监控云智能体任务，并引导会话添加功能。高对比度功能完成后，接下来将[迭代 Copilot 创建的拉取请求][next-lesson]。

## 资源

- [Copilot 智能体页面][agents-page]
- [关于自定义智能体][custom-agents]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
