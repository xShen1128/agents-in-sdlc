---
title: "练习 3 - 自定义智能体"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## 什么是自定义智能体？

GitHub Copilot [自定义智能体][custom-agents-concept]是面向开发工作流中特定任务或领域的专用 AI 助手。你可以在存储库的 `.github/agents` 文件夹中使用 Markdown 文件定义智能体，并为它提供聚焦的指令、最佳实践、编码模式和领域知识。团队可以把专业经验封装为可复用智能体，例如负责执行 [WCAG][wcag] 标准的无障碍智能体、遵循安全编码实践的安全智能体，或维护一致测试模式的测试智能体。

项目级自定义智能体由 `.github/agents` 文件夹中的 Markdown 文件定义，也可以在全局 `~/.copilot/agents` 中定义。每个文件都包含 YAML frontmatter，其中至少要有 `name` 和 `description`，之后是描述智能体行为、专业知识和指令的 Markdown 提示词。

### 自定义智能体与智能体技能的区别

自定义智能体与[智能体技能][agent-skills-concept]有一些相似之处：两者通常都由 Markdown 文件定义，并指导 AI 执行任务。可以将自定义智能体视为工作者，将技能视为工具。

自定义智能体有自己的上下文窗口，并可在工作时协调技能甚至其他智能体。本实验中的无障碍自定义智能体会根据无障碍指南检查并更新网站；它可以调用拉取请求工作流技能或运行、管理测试的技能。

> [!NOTE]
> 编写自定义智能体没有唯一正确的方法。与 AI 的其他用法一样，应在自己的环境和场景中进行测试和迭代。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

本课将介绍：

- 如何定义自定义智能体。
- 如何将任务分配给自定义智能体。

## 场景

Tailspin Toys 希望其众筹平台能为不同视觉能力和偏好的用户提供无障碍体验。近期用户反馈指出，当前暗色主题的文字与背景对比度不足，难以阅读。设计团队因此要求添加可切换的高对比度模式。

无障碍至关重要，因此将使用自定义智能体快速生成此功能。

## 查看无障碍自定义智能体

存储库中已经有无障碍自定义智能体。返回 codespace 查看它的内容：

1. 打开 `.github/agents/accessibility.md`。
2. 查看其中包含 `name` 和 `description` 字段的 YAML frontmatter。

> [!CAUTION]
> 自定义智能体必须包含 `name` 和 `description` frontmatter。

3. 浏览后续部分，了解其中列出的职责、无障碍最佳实践、HTML、CSS 和 JavaScript 示例，以及常见问题。

## 创建议题并分配任务

Mission control 是管理环境中所有智能体的中心位置。你可以在此将任务分配给 Copilot 云智能体、监控任务并提供额外指导。现在创建高对比度模式任务：

1. 打开存储库，选择 **Issues**，再选择 **New issue** 和 **Blank issue**。
2. 将 **Title** 设置为 `Add high contrast mode to website`。
3. 将 **Description** 设置为：

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

4. 选择 **Create**，然后在右侧选择 **Assign to Copilot**。
5. 在列表中选择 **Accessibility agent**，再选择 **Assign**。

    ![在云智能体分配界面中突出显示自定义智能体和 Accessibility 的截图](../../../_images/ex5-select-custom-agent.png)

Copilot 会在后台开始处理任务。

## 总结与下一步

本课介绍了 GitHub Copilot 的[自定义智能体][custom-agents]。使用自定义智能体可以把团队的专业知识和标准封装为可复用智能体，指导 Copilot 更有效地完成特定任务。

你已了解如何定义自定义智能体，以及如何将任务分配给它。Copilot 正在实现高对比度模式，接下来可以从 mission control [监控并引导智能体会话][next-lesson]。

## 资源

- [关于自定义智能体][custom-agents]
- [在组织中使用自定义智能体的准备工作][org-custom-agents]
- [在企业中使用自定义智能体的准备工作][enterprise-custom-agents]

---

| [← 上一课：GitHub Copilot 云智能体][previous-lesson] | [下一课：监控和管理智能体 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
