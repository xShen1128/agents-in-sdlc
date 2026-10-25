---
title: "练习 5：迭代改进 GitHub Copilot 的工作"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：管理智能体][previous-lesson]

## 审查工作

在本学习路径中，你让 GitHub Copilot 完成了多项改善用户体验和添加功能的任务：为代码添加文档、为设计团队实现相关游戏功能，以及通过高对比度和浅色模式改进无障碍体验。现在检查这些更改，必要时向 Copilot 提供反馈。

### 场景

无论是否使用生成式 AI，软件设计和 DevOps 的基本原则都不变。始终需要审查生成的代码，并遵循正常的 DevOps 流程。现在先检查 Copilot 生成的文档、相关游戏功能和无障碍功能，然后再为团队启用审查。

## GitHub Copilot 云智能体安全机制

云智能体会异步且自主地执行任务，因此系统采取了一些安全限制：

- Copilot 对存储库只有读取权限，只能写入用于提交代码的分支。
- 云智能体在 GitHub Actions 中运行，并在独立、临时的环境中工作。
- 任何 GitHub Actions 工作流都必须经过人工批准才能运行。
- 默认情况下，[外部资源访问受到限制](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)，MCP 服务器也包括在内。

## 审查生成的文档

先通过 GitHub.com 的标准 PR 界面查看 Copilot 创建的第一个 PR，其中包含代码文档。

> [!NOTE]
> 查看 PR 时，可能会看到 GitHub Copilot 被防火墙阻止的警告。这是预期行为，因为 Copilot 默认只能有限地访问外部资源，包括外部 MCP 服务器。你也可以[自定义或禁用云智能体防火墙][agent-firewall]。

1. 在 github.com 上打开存储库。
2. 选择 **Pull Requests**，打开拉取请求列表。
3. 打开标题类似 **Add missing documentation** 或更具体的 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成任务；这可能需要几分钟，你可以趁此休息片刻或回顾所学内容。

4. 选择 **Files changed** 选项卡并审查更改。

    ![Files changed 选项卡](../../../_images/shared-pr-files-changed.png)

5. 查看新添加的代码文档。具体更改可能不同。

    留意 TSDoc 文档注释、TypeScript 约定和文件头注释；这些要求来自练习 2 中查看的自定义指令文件。

6. 确认更改符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 工作流会在 PR 的检查区域排队运行。对于单一 Astro 应用，项目检查应当通过；这可能需要几分钟。

## 请求 GitHub Copilot 修改

你可以像在 PR 中评论其他团队成员一样，使用评论或代码行内评论提及 Copilot。Copilot 会读取评论，并启动新会话来处理反馈。结果具有非确定性，因此无法指定唯一的提示词。可以要求 Copilot：

- 为每个代码文件添加说明用途的文件头注释。
- 为 TypeScript 和 Astro 文件添加 TSDoc 文档注释。
- 创建说明 Astro 应用结构的 README。

1. 在 PR 中添加评论并提及 **@copilot**，请求修改生成的文档。可采用上述建议，也可提出其他文档要求。
2. 选择 **View Session**，观察 Copilot 如何启动新会话并处理更改。
3. 选择 **Back to pull request** 返回 PR。

    ![返回拉取请求](../../../_images/ex7-back-to-pr.png)

4. Copilot 完成更改后，PR 中会出现新的提交。选择 **Files changed** 并审查更新。

可以继续迭代，直到结果符合要求。之后可将 PR 从草稿状态标记为可审查，再合并到主分支。

![将 PR 标记为可审查](../../../_images/ex7-ready-for-review.png)

## 审查相关游戏功能

接下来查看用于实现游戏详情页相关游戏的 PR：

1. 返回 GitHub.com 上的存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Show related games on the game details page** 的 PR，或标题更具体的类似 PR。
4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 确认更改符合要求后，返回 **Conversation** 选项卡并向下滚动。
6. 查看是否有工作流等待批准。
7. 如果有，选择 **Approve and run workflows**。

    ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. 确认工作流已在 PR 的检查区域排队运行，并等待单一 Astro 应用的项目检查通过；这可能需要几分钟。
9. **可选：** 可在 Codespace 中手动测试相关游戏功能。打开终端，将 `<branch-name>` 替换为 Copilot 创建的分支名（例如 **copilot/fix-8**），然后运行：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot 已构建相关游戏功能。你可以继续通过 PR 评论请求更改，例如调整显示的相关游戏数量，或补充文件头和 TSDoc 注释。请注意，此任务是在更新自定义指令之前分配的。Copilot 会读取 **Conversation** 选项卡上的新评论并启动新会话。

## 审查无障碍功能

最后，查看使用无障碍自定义智能体实现的功能。PR 应包含练习 3 中要求的高对比度模式，以及练习 4 中通过 mission control 补充的浅色模式。

1. 在 GitHub.com 上打开存储库。
2. 选择 **Pull Requests** 选项卡。
3. 打开标题类似 **Add high contrast mode to website** 的 PR，或标题更具体的类似 PR。

> [!NOTE]
> 如果 Copilot 仍在处理任务，PR 标题会带有 **[WIP]** 标记。请等待 Copilot 完成任务；这可能需要几分钟。

4. 选择 **Files changed** 选项卡，审查生成的代码。
5. 查看实现时，重点关注：
   - 用于切换模式的 UI 组件。
   - 使用本地存储保存用户偏好设置的代码。
   - 高对比度和浅色模式的 CSS 或样式更改。
   - ARIA 属性、键盘导航等无障碍属性。
   - 管理模式切换的 JavaScript 或 TypeScript 代码。
6. 确认更改符合要求后，返回 **Conversation** 选项卡并向下滚动。
7. 查看是否有工作流等待批准。
8. 如果有，选择 **Approve and run workflows**。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. 确认工作流已排队运行，并等待项目检查通过。
10. **可选：** 可在 Codespace 中检出分支并手动测试无障碍功能：

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

启动应用，在浏览器中测试高对比度和浅色模式开关，并确认页面重新加载后设置仍然有效。如果发现无障碍问题或改进机会，可像之前一样在评论中提及 **@copilot** 并请求更新。

## 可选练习 — 继续委派

云智能体适合处理真实待办事项，让你可以专注于其他工作。可以在存储库中创建更多议题并分配给 Copilot，例如：

- 在游戏详情页创建支持者兴趣表单。
- 为游戏列表页添加分页。
- 为数据访问助手添加输入验证和错误处理。

## 总结

你已完成云智能体学习路径。整个过程中，你查看了项目的自定义指令，向 Copilot 云智能体分配议题并观察其异步工作，创建并使用无障碍自定义智能体，通过 Copilot 智能体页面监控和引导会话，并审查 PR、提及 `@copilot` 请求更改及批准工作流。

## 回顾与后续步骤

如果希望继续探索，可尝试其他学习路径：

- 🖥️ **[VS Code 学习路径](../../vscode/)** — 在 IDE 中体验 Copilot Chat 智能体模式和 MCP 集成。
- 💻 **[CLI 学习路径](../../cli/)** — 在终端中交付两项经过审查的更改，使用 Plan 和 Autopilot 模式、智能体技能、自定义 QA 智能体、Playwright MCP、Agent Merge，以及可选的 `/delegate` 云智能体桥接。

在自己的存储库中，还可以尝试：

- 将重构或测试覆盖率议题分配给云智能体。
- 为其他领域创建自定义智能体。
- 为其他技术栈设置 `copilot-setup-steps.yml`。

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
