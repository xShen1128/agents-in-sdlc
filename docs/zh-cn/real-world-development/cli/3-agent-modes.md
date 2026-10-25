---
title: "第 3 课 - 智能体模式：Plan 和 Autopilot"
description: "使用 Plan 商定方案，用 Autopilot 根据议题实现筛选功能，再通过 Interactive 模式审查结果。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

我们已在项目中添加了一个小功能，但较大的更改需要更完善的流程。GitHub Copilot CLI 可以融入组织现有的工作方式，帮助我们正确地构建所需功能。本课将开始一系列基于智能体的开发练习：从议题实现新功能，确保代码有效、功能符合预期，最后成功合并到项目中。

> [!NOTE]
> 后续功能开发课程会继续使用同一个对话和分支。通常，不同文件类型的工作会使用不同分支或 PR；本工作坊为聚焦核心概念而简化了流程。

本课将学习：

- 根据 GitHub 议题开始新的 Copilot 对话。
- 在 Plan 模式中明确需求。
- 使用 Autopilot 模式实现新功能。
- 审查代码。
- 在转发的浏览器中手动验证功能。

之后还会更新存储库指令、自定义现有的 `quality-checks` 技能、添加 MCP 验证、创建 QA 智能体并创建功能 PR。

## 场景

Tailspin Toys 的目录不断扩大，访问者需要按类别和发行商筛选游戏。待办事项中的议题描述了功能，但在编写代码前，还要商定如何组合类别等细节。你将使用 Plan 模式明确这些决定，再使用 Autopilot 完成范围明确的实现。

## 背景

在开发流程中引入 AI 编码智能体并不会改变基本原则，反而让这些原则更加重要。大多数开发人员的流程大致如下：

1. 从描述需求的已创建议题开始。
2. 制定构建方案。
3. 编写并审查代码。
4. 运行测试以验证代码。
5. 手动验证新功能。
6. 创建拉取请求 (PR)。
7. 代码经过审查且持续集成成功后，合并代码。

> [!NOTE]
> 具体流程会因团队和组织而异，但大多与上述步骤类似。

遵循标准流程，可确保 AI 生成的代码符合需求，并经过与手写代码相同的审查流程。

## 对话模式

**对话模式**决定智能体的自主程度。按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 可在模式间切换：

- **Interactive**：你与智能体协作；智能体提出更改建议，并等待你输入后再继续。
- **Plan**：智能体先制定计划，但不能编辑项目文件。
- **Autopilot**：智能体自主编写代码、运行测试并迭代，直到认为任务已完成。

先使用 Plan 模式审查计划，再切换到 Autopilot 实现。

## 从议题开始

开始筛选功能前，返回 Codespace 并确认存储库和终端已准备就绪：

1. 返回 Codespace；如果它已停止，请先重启。
2. 确认星级评分 PR 已合并。
3. 如果终端尚未打开，按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）。
4. 更新 `main`，并为筛选功能创建分支：

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. 启动 Copilot CLI：

   ```bash
   copilot --yolo
   ```

6. 按两次 <kbd>Tab</kbd> 打开 **Issues** 选项卡。
7. 按 <kbd>A</kbd> 显示所有议题。
8. 使用方向键突出显示 **Allow users to filter games by category and publisher**。
9. 按 <kbd>C</kbd> 将议题添加到提示词中，并返回 **Session** 选项卡。

提示词现在会以 `#7`（或类似编号）开头。`#` 可将 GitHub 议题或拉取请求 (PR) 添加到上下文中。

## 规划筛选功能

规划能让你在交给 Copilot 实现前，先定义功能方案。对于复杂工作，花些时间规划通常很有帮助。现在切换到 Plan 模式并要求 Copilot 制定计划：

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 切换到 Plan 模式，确认提示词下方显示 **Plan**。
2. 在上一步添加的议题引用后输入：

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot 会探索项目并确定适当方案。它可能会询问筛选功能应如何工作；请根据自己的偏好回答，没有唯一正确答案。

3. 计划完成后，按 <kbd>Control</kbd>+<kbd>E</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>E</kbd>（Windows/Linux）展开计划。
4. 查看计划，并要求 Copilot 修改不符合你决定的部分。

## 批准 Autopilot

计划编写并审查完成后，就可以开始实现。Autopilot 会让 Copilot 反复处理问题，直到它认为已完成。

1. 选择 **Accept plan and build on autopilot (recommended)**，或当前版本中含义相同的选项。
2. 确认提示词下方显示 **Autopilot**。
3. 观察 Copilot 如何按计划迭代、生成代码并运行测试。

> [!NOTE]
> 批准计划后可能会立即开始实现，因此务必先审查计划。如果 Copilot 报告缺少依赖项或端口冲突，应先解决环境问题，再判断检查结果。

## 审查并验证实现

与其他代码一样，生成的代码也必须在合并前经过审查。检查代码并运行网站，确认结果符合预期：

1. 按 <kbd>Shift</kbd>+<kbd>Tab</kbd> 进入 Interactive 模式，确认提示词下方不再显示 **Plan** 或 **Autopilot**。
2. 输入 `/diff`，检查筛选功能的实现和测试。
3. 根据议题和规划阶段确定的内容审查结果。
4. 审查完成后按 <kbd>Esc</kbd> 退出差异视图。
5. 查看项目检查结果，并要求 Copilot 解决失败项。

## 探索新功能

代码看起来没问题，还需要实际运行验证。像之前一样启动应用，并通过 Codespaces 转发端口打开网站：

1. 请求 Copilot 启动应用：

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces 报告端口 `4321` 可用时，选择 **Open in Browser**。
3. 尝试按类别和发行商筛选，以及规划阶段商定的组合方式。
4. 确认重置和无结果状态符合议题及商定的行为。
5. 返回 Codespace，并请求 Copilot 停止它启动的开发服务器。

## 总结与下一步

本课使用不同的对话模式构建并审查功能。你从 GitHub 议题开始对话，在 Plan 模式中明确需求，使用 Autopilot 实现功能，审查代码，并在转发的浏览器中手动验证。

接下来，使用[自定义指令][next-lesson]深入了解如何按已记录的实践生成代码。

## 资源

- [GitHub Copilot CLI 中的 Autopilot][autopilot]
- [Copilot CLI 命令参考][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
