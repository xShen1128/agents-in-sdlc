---
title: "练习 5 - 监控和管理智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 上一课：自定义智能体][previous-lesson] |
|:--|

使用 GitHub Copilot 智能体模式时，它会自主探索代码库、提出更改、编辑文件并运行命令。由于这些操作会在计算机上实时进行，VS Code 中的 Copilot Chat 会显示每次工具调用和文件编辑。你可以内联审查差异、接受或拒绝单项更改，并通过后续提示引导或扩展工作，全程无需离开编辑器。

本练习将介绍如何实时监控无障碍智能体、审查并接受建议的文件编辑，以及在会话中要求它同时添加浅色模式开关和高对比度模式。

## 场景

[练习 4][previous-lesson]中使用的无障碍自定义智能体正在实现高对比度模式。工作进行期间，设计团队提出了新要求：为觉得默认主题太暗的用户添加浅色模式开关。无需从头开始新对话，可以引导当前会话扩展工作。

## 监控 Copilot 进度

Copilot Chat 视图会显示智能体执行的每一步，包括探索的文件、运行的命令和提出的编辑。每次文件编辑都会在聊天中内联显示差异，以及 **Keep** 和 **Undo** 控件。

1. 返回练习 4 中的 Copilot Chat 视图。
2. 智能体工作时滚动查看工具调用，包括文件读取、文件编辑和请求运行的终端命令。
3. 智能体提出文件编辑时，差异会内联显示：
   - 选择 **Keep** 接受更改。
   - 选择 **Undo** 撤销更改，让智能体再尝试一次。
4. 也可以打开聊天顶部的 **Files changed** 指示器，查看此会话中已修改的所有文件。

> [!NOTE]
> 如果工具调用需要权限（例如运行终端命令），智能体会暂停并显示 **Allow** 提示。只批准你确认可以执行的命令。

5. 等待智能体完成高对比度模式，并查看它对所做更改的总结。

## 引导智能体添加浅色模式

高对比度模式完成后，在同一个对话中要求添加浅色模式。由于继续使用同一会话，智能体会保留之前工作的上下文，包括涉及的组件、样式方案和本地存储实现方式。

1. 在 Copilot Chat 输入框中发送以下后续提示词：

    ```plaintext
    Nice work! Now add a light-mode toggle alongside the high-contrast toggle. Light mode should follow the same pattern — persist via local storage and be accessible from the same area of the UI. The user should be able to enable high-contrast and light mode independently.
    ```

2. 智能体会查看已有更改，并提出浅色模式的新增内容。
3. 像审查高对比度模式一样审查每项新编辑，逐项选择 **Keep** 或 **Undo**。
4. 如果智能体偏离预期（例如重构开关 UI 的方式不符合要求），可选择聊天输入框旁的 **Stop** 按钮停止它，然后发送澄清提示。

> [!TIP]
> 引导正在进行的会话比从头开始更快。可以通过后续提示要求调整，例如“简化 CSS，使用 CSS 变量而不是重复选择器”；纠正方向，例如“不要添加第三个开关，将浅色模式集成到现有设置面板”；或扩展范围，例如“为两个开关添加键盘快捷键”。

## 审查更改集

提交更改前，快速检查智能体修改的所有内容：

1. 在 VS Code 中打开 **Source Control** 视图。
2. 查看全部已更改文件，其中应包括 Astro 组件、样式和相关测试。
3. 打开几个已更改文件并查看差异。确认代码体现了自定义智能体提出的无障碍模式，包括 ARIA 属性、键盘导航、语义化 HTML 和通过本地存储保存设置。
4. 确认更改后，在 Source Control 面板中暂存并提交。下一课会发布这些更改。

## 总结与下一步

你使用 Copilot Chat 智能体模式监控并引导进行中的会话：逐项审查文件编辑、接受或拒绝更改，并通过后续提示扩展工作，同时保留已有上下文。

本课中，你实时监控智能体工作、接受或拒绝单项文件编辑，并通过后续提示引导会话。下一课将把无障碍改进发布为拉取请求，并审查本实验中本地自定义智能体生成的更改。

## 资源

- [VS Code 中的 Copilot Chat][copilot-chat-vscode]
- [使用智能体模式][agent-mode]

---

| [← 上一课：自定义智能体][previous-lesson] | [下一课：迭代 Copilot 的工作 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../4-custom-agents/
[next-lesson]: ../6-iterating/
[copilot-chat-vscode]: https://code.visualstudio.com/docs/copilot/chat/copilot-chat
[agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
