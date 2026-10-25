---
title: "练习 2 - 使用 Copilot 智能体模式添加功能"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

即使是简单的应用更新，通常也需要修改多个文件并运行测试。开发人员往往需要查找相关文件、实施更改、运行测试、调试、找出遗漏文件并再次修改。Copilot 智能体模式可以协助完成这些工作。

Copilot 智能体模式可在 IDE 中更自主地工作：先探索项目结构，再执行必要更新、运行测试等任务，并自动修复发现的问题。现在了解如何使用它为网站添加功能。

> [!NOTE]
> 智能体模式和云智能体虽然名称相似，但面向不同体验。智能体模式在 IDE 中执行任务，可快速反馈并进行交互；云智能体则像团队成员一样异步工作，通过议题和拉取请求与你协作。

本练习将介绍如何让 Copilot 智能体模式探索项目、定位相关文件、协调 UI 和数据层更改，并在合并前审查 Copilot 生成的更改和测试。

## 场景

游戏列表不断增长，你希望用户能按类别和发行商筛选。上一练习中已经添加发行商助手；现在将使用 Copilot 智能体模式完成剩余的数据层、UI 和测试工作。

## 运行 Tailspin Toys 网站

更改代码前，先了解网站当前功能。Tailspin Toys 是一个带有开发者主题的桌游众筹平台，用户可以查看游戏列表和详情。该网站是一个单体 Astro 应用，构建时会将页面渲染为静态 HTML。页面通过 Drizzle ORM 在 frontmatter 中直接查询本地 SQLite 数据库，没有单独的后端 API 或客户端 UI 框架。可复用的数据访问助手位于 `src/lib/`，交互性由使用标准 DOM API 的小型 Astro `<script>` 实现。

### 启动网站

项目提供了 `npm run dev` 任务，用于启动单个 Astro 开发服务器：

1. 返回 Codespace。上一练习结束后应位于 `main` 分支。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）在 Codespace 中打开终端。
3. 创建筛选功能分支：

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 启动网站：

   ```bash
   npm run dev
   ```

   Astro 开发服务器准备就绪后，会显示类似以下信息：

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> 如果出现提示，询问是否在浏览器中打开 `http://localhost:4321`，请选择 **x** 关闭对话框。

5. 按住 <kbd>Command</kbd>（Mac）或 <kbd>Ctrl</kbd>（Windows/Linux），然后选择终端中的 Astro 服务器地址 `http://localhost:4321`。

> [!NOTE]
> 在 codespace 中打开 localhost 链接时，会自动重定向到 `https://<your-codespace-name>-4321.app.github.dev/`。这是指向 codespace 中 Web 服务器的专用隧道。

### 探索网站

网站启动后，可以查看主要功能：

- **主页**：显示桌游列表、标题、图片和说明。
- **游戏详情页**：选择游戏后，可查看标题、说明、发行商和类别等详细信息。

## 使用 Copilot 探索待办事项

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace 并在 Copilot Chat 面板中选择 **New Chat**，开始新的对话。模式和模型要求会在适用步骤中说明。

项目目前已能运行，但还需要添加功能。模板会自动创建议题待办列表；让 GitHub Copilot 查看这些议题：

1. 在 Chat 视图的智能体下拉菜单中选择 **Agent**。Agent 智能体会自主规划并修改多个文件、运行终端命令和调用工具。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 从可用模型列表中选择 **Claude Sonnet 4.5**。

> [!CAUTION]
> 工作坊作者并不偏好某个模型。本工作坊使用 Claude Sonnet 4.5，是希望代码建议较为一致、学习体验更顺畅。由于大语言模型具有概率性，收到的建议可能与示例不同，这是正常现象。

> [!NOTE]
> 由于大语言模型具有概率性，Copilot 可能会使用不同的 MCP 命令，但仍应能够完成任务。

3. 向 Copilot 发送以下提示词：

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. 选择 **Continue** 以运行列出所有议题的命令。
5. 查看 Copilot 生成的议题列表和优先级建议。

## 查看指令文件

让智能体生成代码前，先查看为 Copilot 提供项目上下文的指令文件。本练习会使用 [UI 指令文件](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)，其中说明了如何为网站添加功能。

1. 在 codespace 中打开 `.github/instructions/ui.instructions.md`。
2. 查看其中有关功能实现方式的指导，包括架构概述、组件设计、可测试性和无障碍原则，以及 Astro、Tailwind CSS 和 Drizzle 等文件类型对应的指令链接。

> [!TIP]
> 指令文件可以引用其他指令文件和项目文件。路径相对于指令文件所在位置，因此可以复用指导、拆分复杂内容，并提供示例和模板。

## 实现筛选功能

完成筛选至少需要三项更改：

- 在数据层 (`src/lib/`) 中添加或完善筛选逻辑。
- 添加或更新筛选行为测试。
- 更新游戏列表页以添加筛选 UI。

合并前还必须运行并通过测试。Copilot 智能体模式可以协助完成这些工作：

1. 继续当前 Copilot 对话，或选择 **New Chat** 开始新对话。
2. 选择 **Add Context**、**Instructions** 和 `ui` 指令文件。

   ![选择 UI 指令文件的示例截图](../../../_images/ex3-select-instructions-file.png)

3. 在 Chat 视图的智能体下拉菜单中确认已选中 **Agent**。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 确认仍选择 **Claude Sonnet 4.5**。
5. 使用以下提示词，让 Copilot 根据待办事项中的相关议题实现功能：

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. 观察 Copilot 如何探索项目、定位数据层助手、UI 和测试文件，然后修改文件并运行测试。

   ![Copilot 探索项目文件的截图](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot 可能会先探索项目、修改文件并运行测试，整个过程可能需要几分钟。期间编辑器中可能出现 **Keep** 和 **Undo**；等待完成后再选择即可。

7. Copilot 提示时选择 **Continue** 运行测试。

   ![Copilot Chat 窗格中提示用户确认运行测试的对话框截图](../../../_images/ex3-agent-mode-run-tests.png)

8. 过程中可能会暂停，甚至出现测试失败。这并不一定表示任务无法完成；Copilot 会在生成代码和运行测试之间迭代，直到完成任务且不再发现错误。

   ![Copilot 智能体模式完整 Chat 会话的截图](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 检查生成的代码是否存在问题。

> [!CAUTION]
> 始终审查 Copilot 或其他 AI 工具生成的代码。

10. 返回正在运行的网站，探索新功能。
11. 确认功能正常并审查代码后，在 Copilot Chat 窗口中选择 **Keep**。

## 总结与下一步

你已使用 GitHub Copilot 智能体模式为 Tailspin Toys 网站添加功能，了解了它如何探索项目并跨 UI 和数据层实现更改，以及合并前审查生成的更改和测试的重要性。

接下来使用 [Playwright MCP 服务器测试功能][next-lesson]，并创建功能拉取请求。

### 额外探索练习 — 实现分页

游戏列表不断增长后需要分页。运用本练习所学，请 Copilot 为网站添加分页。可考虑遵循现有最佳实践并使用已有指令文件，决定让用户选择每页大小还是使用固定大小，并在提示词中提供所需指导。你可能需要通过后续提示和上下文不断迭代，这是使用 Copilot 的正常流程。

## 资源

- [Copilot 的 Ask、Edit 和 Agent 模式：功能及使用时机][choose-mode]
- [VS Code 中的智能体模式][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
