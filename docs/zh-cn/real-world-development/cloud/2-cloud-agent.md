---
title: "练习 2 - GitHub Copilot 云智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：自定义指令][previous-lesson]

许多组织都面临技术债务，例如尚未解决的问题、需要更新的旧代码，或长期滞留在待办事项中的功能请求。GitHub Copilot 云智能体可以自主执行更新代码和添加功能等任务。完成后，它会创建草稿 PR，供开发人员审查。这样可以委派繁琐任务、加快开发流程，让开发人员专注于更重要的工作。

本课将介绍：

- 如何自定义代码生成环境。
- 如何确保操作安全。
- 清楚界定议题范围的重要性。
- 如何将议题分配给 Copilot。

## 场景

Tailspin Toys 有一些技术债务。最初开发网站的承包商没有为代码添加文档，因此团队希望先为应用中的所有导出函数添加 TSDoc 文档注释。

设计团队还希望改进游戏发现体验：在每个游戏详情页显示同一类别中的其他游戏，供用户继续浏览。当前只需要可用于验收测试 UX 的初步实现。虽然该工作被暂时搁置，但仍有其他优先级更高的议题。

这类任务很容易被延后，适合分配给 Copilot 云智能体。它可以在后台异步处理任务，让开发人员继续做其他工作，之后再回来审查结果。

## 了解 GitHub Copilot 云智能体

[GitHub Copilot 云智能体][copilot-agents]可以像开发人员一样在后台执行任务。你可以通过多种方式分配工作，例如[将 GitHub 议题分配给 Copilot][assign-issue]。分配后，Copilot 会创建草稿拉取请求以跟踪进度，设置环境并开始处理任务。你可以在会话进行期间或完成后查看它的工作；准备好审查方案时，它会在拉取请求中通知你。

## 明确指令和任务范围

GitHub Copilot 并非魔法工具，使用它并不意味着可以跳过思考。即使看似简单的操作也可能包含复杂性，因此应谨慎界定[云智能体任务范围][cloud-agent-best-practices]。应像使用 AI 结对程序员一样分阶段开展工作、学习、试验并不断调整。加入生成式 AI 并不会改变软件开发的基本原则。

## 此存储库中的自定义指令

前面的练习介绍了自定义指令及其作用。如果已完成自定义指令练习，就已经见过 `.github/copilot-instructions.md` 的效果；否则可先快速查看。分配任务前，请以只读方式查看此存储库中的指令文件，之后便能观察它们对云智能体工作的影响。

在 GitHub Web 界面或已启动的 codespace 中查看：

- `.github/copilot-instructions.md`：查看 **Code standards**，尤其是 TypeScript 约定和 TSDoc 文档注释要求。
- `.github/instructions/unit-tests.instructions.md`：查看 `applyTo` frontmatter，它将指令限定到 `**/*.test.ts` 文件。

下一节将把 `Code lacks documentation` 议题分配给云智能体。之后查看生成的 PR，留意 TSDoc 注释、TypeScript 约定和文件头注释；这些要求来自上述指令文件。

## 为 Copilot 云智能体设置开发环境

无论由谁编写代码，通常都需要特定环境和设置脚本。将任务分配给 Copilot 时也一样。

云智能体使用 [GitHub Actions][github-actions] 环境。你可以创建 `.github/workflows/copilot-setup-steps.yml` 特殊设置工作流，在智能体开始工作前运行，以提供所需工具和依赖项。此实验已预先配置该工作流，其中包括 Node.js、项目依赖项、端到端测试所需的浏览器，以及为单一 Astro 应用迁移并填充数据的 SQLite 数据库：

```yaml
name: "Copilot Setup Steps"

# 可从存储库的 "Actions" 选项卡测试这些设置步骤
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 将权限设为这些步骤所需的最低级别。Copilot 会使用自己的令牌执行操作。
    permissions:
      # 如果设置步骤需要克隆存储库（例如安装依赖项），则需要 `contents: read` 权限。如果不在设置步骤中克隆存储库，步骤完成后 Copilot 会自动为你执行此操作。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # 前端/应用设置 — Node.js（整个应用现为 Astro + Drizzle/libSQL）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # 迁移并填充本地 SQLite 数据库，为构建和测试准备数据。
      - name: Set up the database
        run: npm run db:setup
```

这与其他 GitHub 工作流文件类似，但有几点需要注意：

- 其中只有一个名为 `copilot-setup-steps` 的作业。Copilot 开始处理 PR 前，GitHub Actions 会运行此作业。
- `workflow_dispatch` 允许你从存储库的 **Actions** 选项卡手动运行工作流，以便在等待 Copilot 之前验证配置。

## 添加代码文档

许多项目缺少文档或文档已经过时，形成难以维护代码库、降低工作效率并阻碍新成员加入的技术债务。Copilot 擅长生成文档，因此可以将此任务分配给云智能体，稍后再审查其工作。

1. 在新的浏览器标签页中打开 github.com 上的存储库。
2. 选择 **Issues** 选项卡，再选择 **New issue**。
3. 选择 **Blank issue**。
4. 将 **Title** 设置为 `Code lacks documentation`。
5. 将 **Description** 设置为：

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

6. 选择 **Create** 创建议题。
7. 在右侧选择 **Assign to Copilot**，然后选择 **Assign**。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

8. 打开 **Pull Requests** 选项卡，查看新建的 PR。标题可能类似 `[WIP]: Code lacks documentation`。如果尚未显示，请稍等片刻并刷新页面。
9. 等待 Copilot 创建待办事项列表，然后检查它准备执行的任务。Copilot 会创建环境、运行前面介绍的设置工作流、分析项目并确定解决问题的方案，因此可能需要几分钟。
10. 在 PR 时间线上查看 Copilot 的进度。你应该会看到 Copilot 已开始处理议题，然后选择 **View session**。

    ![Copilot 会话视图](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 若要看到最新状态，可能需要刷新页面。

11. 查看实时会话。你可以滚动查看 Copilot 如何探索代码并了解当前状态，也可以观察它如何暂停思考、确定合适的方案并创建代码。

此任务可能需要几分钟。云智能体的主要优势之一是能够异步执行任务，让你可以同时处理其他工作。接下来利用这一点，再分配另一项任务，然后自己继续编写应用功能。

## 为游戏详情页添加相关游戏

云智能体可以并行处理工作，让你专注于其他任务。现在将设计团队的相关游戏需求也分配给它：

1. 返回 github.com 上的存储库，选择 **Issues**。
2. 选择 **New issue** 打开新议题对话框，再选择 **Blank issue**。
3. 将 **Title** 设置为 `Show related games on the game details page`。
4. 将 **Description** 设置为：

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

5. 选择 **Create** 创建议题。
6. 在右侧选择 **Assign to Copilot** 打开分配对话框，再选择 **Assign**。

    ![将 Copilot 分配给议题](../../../_images/shared-assign-copilot.png)

不久后，议题第一条评论中会出现 👀，表示 Copilot 已开始处理。Copilot 云智能体的工作方式与软件工程师类似；无需主动监控，等任务完成后再审查即可。接下来将学习创建和使用自定义智能体。

## 总结与下一步

本课介绍了 [GitHub Copilot 云智能体][copilot-agents]。你可以将议题分配给 Copilot 异步处理，以解决技术债务、创建功能，或协助迁移代码。

你已了解如何自定义代码生成环境、安全地配置操作、界定议题范围并将任务分配给 Copilot。云智能体还支持 [MCP 服务器][cloud-agent-mcp]，并会使用前面练习中配置的自定义指令。

接下来，[创建并使用自定义智能体][next-lesson]。

## 资源

- [关于 Copilot 云智能体][copilot-agents]
- [将 GitHub 议题分配给 Copilot][assign-issue]
- [Copilot 云智能体设置工作流最佳实践][cloud-agent-best-practices]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
