---
title: "练习 4 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot [自定义智能体][custom-agents-concept]是面向开发工作流中特定任务或领域的专用 AI 助手。通过在存储库的 `.github/agents` 文件夹中定义 Markdown 文件，可以为 Copilot 提供聚焦的指令、最佳实践、编码模式和领域知识，帮助它更有效地完成特定任务。团队可以将专业经验封装为可复用智能体，例如执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

自定义智能体由项目 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，其中至少有 `name` 和 `description`，随后是定义智能体行为、专业知识和指令的 Markdown 提示词。

### 自定义智能体与智能体技能的区别

自定义智能体和[智能体技能][agent-skills-concept]都由 Markdown 文件定义，并指导 AI 执行任务。最简单的区分方式是：**自定义智能体是工作者，技能是工具**。

自定义智能体有自己的上下文窗口，并可在工作时协调技能，甚至协调其他智能体。本练习中的无障碍自定义智能体会根据无障碍指南审查并更新网站；它可以调用拉取请求工作流技能或运行、管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，应在自己的环境和场景中测试并迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

## 场景

Tailspin Toys 希望众筹平台对不同视觉能力和偏好的用户都无障碍。近期用户反馈指出，当前暗色主题的文字和背景对比度不足，难以阅读。设计团队因此要求添加可由用户切换的高对比度模式。

无障碍至关重要，因此将使用自定义智能体生成此功能。

本练习将介绍如何查看已有的无障碍自定义智能体，以及如何在 Copilot Chat 中使用它实现高对比度模式。

## 查看无障碍自定义智能体

存储库已提供无障碍自定义智能体。返回 codespace 查看它如何引导 Copilot：

1. 打开 `.github/agents/accessibility.md`。
2. 查看其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解生成无障碍网站代码时的核心职责、无障碍最佳实践、HTML、CSS 和 JavaScript 示例，以及常见问题。

## 在 Copilot Chat 中使用自定义智能体

VS Code 会在 Copilot Chat 视图底部的智能体下拉菜单中显示 `.github/agents` 中定义的所有自定义智能体。选择自定义智能体后，会话会使用该智能体的指令和工具。

> [!TIP]
> **打开 Copilot Chat**
>
> 开始以下练习前，返回 codespace 并在 Copilot Chat 面板中选择 **New Chat**，开始新的对话。模式和模型要求会在适用步骤中说明。

1. 如果 Chat 视图中尚未选中 **Agent**，请在智能体下拉菜单中选择它。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

2. 选择 Chat 视图底部显示当前智能体名称的下拉菜单。默认情况下，当前智能体为 **default**。
3. 从列表中选择 **Accessibility agent**。
4. 向无障碍智能体发送以下提示词：

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat 会探索代码库、提出更改并修改项目文件。每项编辑都会在聊天中内联显示文件路径和差异，供你审查。
6. 智能体修改文件时，**Files changed** 指示器会更新，显示已更改文件。

> [!NOTE]
> 这个过程可能需要几分钟。Copilot 会修改 Astro 组件、CSS 和相关测试等现有文件。

下一课将审查并引导正在进行的工作。

## 总结与下一步

本课介绍了 GitHub Copilot 的[自定义智能体][custom-agents]。使用自定义智能体可以把团队的专业知识和标准封装为可复用智能体，指导 Copilot 更有效地完成特定任务。

你已了解如何定义自定义智能体，以及如何在 Copilot Chat 智能体模式中使用它。接下来[监控并引导智能体工作][next-lesson]，审查逐步生成的更改，并在同一会话中添加浅色模式开关。

## 资源

- [关于自定义智能体][custom-agents]
- [在 IDE 中创建自定义智能体][creating-custom-agents-ide]
- [VS Code 中的自定义智能体][custom-agents-vscode]
- [自定义智能体配置][custom-agents-config]
- [Awesome Copilot 中的自定义智能体][awesome-copilot-agents]

---

| [← 上一课：使用 Playwright MCP 服务器测试功能][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
