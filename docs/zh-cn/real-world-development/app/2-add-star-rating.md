---
title: "第 2 课 - 添加星级评分：快速上手"
description: "在 GitHub Copilot app 中启动第一个智能体会话，对游戏卡片进行一项小改动，并通过第一个拉取请求合并更改。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

你已了解工作区并试用过快速聊天，现在可以启动**智能体会话**并对项目进行第一次更改。这次改动很小：游戏数据中已有星级评分，但主页上的游戏卡片尚未显示。让智能体显示评分、审查更改，并通过第一个拉取请求合并更改。

本课将学习：

- 启动智能体会话并了解会话结构。
- 要求智能体对项目进行一项范围明确的小改动。
- 在工作区差异视图中审查更改。
- 在本地运行应用，并在浏览器中确认更改。
- 创建并合并第一个拉取请求。

## 场景

Tailspin Toys 中的每款游戏都可以有星级评分，而且游戏详情页已经显示该评分。但主页上的游戏卡片只显示标题、类别、发行商和说明。作为热身练习，让智能体在每张卡片上显示现有评分。这项小型、独立的更改非常适合作为第一个会话任务。

## 会话的组成

**会话**是与智能体的对话，在独立工作区中运行。每个会话都有**专用的 git 工作树和分支**，因此可以同时运行多个会话，例如一个添加功能，另一个修复 bug，而不会造成更改冲突。会话按存储库分组显示在侧边栏中，选择即可切换。

会话中会显示三类内容：与智能体的**对话**、智能体探索并编辑文件时执行的**工具操作**，以及带有差异的**已更改文件**列表。

## 启动会话并请求更改

现在启动新会话以探索项目并实现功能。在[应用设置][prior-lesson]中，你已通过 GitHub 存储库添加项目。现在为该存储库创建新会话并请求更改：

1. 返回（或打开）GitHub Copilot app。
2. 选择侧边栏顶部的 **+**，启动会话。
3. 为存储库选择 `tailspin-toys`。
4. 在提示词框下方选择**新工作树**和 **Interactive** 模式。输入以下提示词：

    ```plaintext
    Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
    ```

5. 按 <kbd>Enter</kbd> 将提示词发送给 Copilot。

Copilot app 会先创建新的工作树，即项目的隔离副本，然后探索项目并找到需要更新的文件，再创建必要代码。现在，你已使用 Copilot app 添加一项新功能。

## 审查差异

所有 AI 生成的更改在合并前都应审查，即使改动很小。现在直接在 Copilot app 中检查：

1. 在应用右上角选择 **Toggle review panel**。打开面板后选择 **Changes**，查看已更改文件及其差异。

    ![GitHub Copilot app 顶部工具栏，箭头指向 Create PR 右侧的 Toggle review panel 按钮](../../../_images/app-2-review-panel.png)

2. 检查 `GameCard.astro` 中新增的代码。它是用于显示游戏详情的核心文件，代码应类似以下示例：有评分时显示评分，`starRating` 为 `null` 时显示 "No rating yet"：

   ```astro
   {game.starRating !== null ? (
       <span class="text-xs font-medium px-2.5 py-0.5 rounded bg-amber-900/60 text-amber-300" data-testid="game-rating">
           ★ {game.starRating} / 5
       </span>
   ) : (
       <span class="text-xs font-medium text-slate-500" data-testid="game-rating-empty">
           No rating yet
       </span>
   )}
   ```

> [!NOTE]
> Copilot 与其他生成式 AI 工具一样具有概率性而非确定性，实际代码可能与示例不同，但应大致相似。

## 检查更改

打开浏览器前，先审查智能体自动运行的检查结果。确认测试覆盖数值类型的 `starRating` 和 `null` 回退状态。缺少先决条件或跳过检查都不能算通过；批准前请审查安装请求。

不能只阅读代码并假设它能正常运行。让 Copilot 启动网站，并在浏览器画布中查看更新后的 UI：

> [!TIP]
> 画布是 Copilot app 内的交互式小组件。稍后你将探索自定义画布，甚至创建自己的画布；现在先使用内置的浏览器画布。

1. 使用以下提示词，让 Copilot 启动应用并在浏览器画布中打开页面：

    ```plaintext
    Start the app and open it in the browser canvas.
    ```

2. 稍等片刻，应用会启动，Copilot app 内会打开浏览器窗口。
3. 确认已评分的游戏卡片显示五分制评分。
4. 完成后，使用以下提示词让 Copilot 停止为此会话启动的开发服务器并关闭浏览器画布：

    ```plaintext
    Stop the dev server and close the browser canvas.
    ```

## 创建并合并第一个拉取请求

功能已创建，现在可以创建拉取请求 (PR)，将代码合并到项目中：

1. 选择右上角的 **Create PR**。
2. 如果系统提示，请选择 **Sign in with your browser**，并按提示完成身份验证。
3. 如果 Copilot 说明存储库策略要求先关联议题才能创建 PR，请确认它应遵循该流程。
4. Copilot 会开始创建 PR。
5. 选择聊天上方的 **PR** 气泡，在审查窗格中打开并查看 PR。
6. 准备好后，选择 **Ready to merge**。
7. 在新对话框中选择 **Merge pull request**，合并拉取请求。

## 总结与下一步

恭喜你使用 GitHub Copilot app 交付了第一个更改。你启动了智能体会话、让智能体对游戏卡片做小幅且明确的更改、在工作区差异视图中审查更改、在本地运行应用并通过浏览器确认星级评分，还创建了 PR 1、审查检查结果并明确合并。

接下来，从筛选功能议题开始，使用 Plan 和 Autopilot 模式[构建更大型的功能][next-lesson]。

## 资源

- [在 GitHub Copilot app 中使用智能体会话][agent-sessions]
- [关于 GitHub Copilot app][about-copilot-app]
- [使用 GitHub Copilot app 管理议题和拉取请求][managing-issues-prs]

[prior-lesson]: ../1-install-copilot-app/#安装并配置-github-copilot-app
[next-lesson]: ../3-agent-modes/
[agent-sessions]: https://docs.github.com/copilot/how-tos/github-copilot-app/agent-sessions
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[managing-issues-prs]: https://docs.github.com/copilot/how-tos/github-copilot-app/managing-issues-and-pull-requests
