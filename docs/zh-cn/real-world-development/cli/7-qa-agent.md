---
title: "第 7 课 - 创建并使用 QA 智能体"
description: "创建 QA 自定义智能体，结合议题需求、quality-checks 技能和 Playwright MCP。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

你已使用 `quality-checks` 技能运行自动化检查，并使用 Playwright MCP 在浏览器中观察筛选体验。现在将这些能力整合到具有明确 QA 流程的自定义智能体中。

本课将学习：

- 了解自定义智能体如何使用指令、技能和 MCP 工具。
- 创建并检查可复用的质量保证配置文件。
- 选择 QA 智能体，根据筛选议题审查其发现。

## 场景

Tailspin Toys 希望在创建拉取请求 (PR) 前，以一致的方式审查需求、代码质量、自动化检查、测试覆盖率和浏览器行为。自定义智能体可以协调 QA 流程并生成可复用报告。

## 什么是自定义智能体？

自定义智能体是通过 Markdown 配置文件定义的 Copilot 专用版本。配置文件说明智能体的用途、指令和可用工具。本工作坊将在 `.github/agents/qa.agent.md` 中定义 QA 角色，并在 Copilot CLI 中选择它。

不同自定义项各有用途：存储库指令描述团队标准，`quality-checks` 技能封装可重复检查，Playwright MCP 提供浏览器工具，QA 配置文件则告诉 Copilot 如何使用这些能力评估需求并报告发现。QA 配置文件不会取代这些工具，也不需要另开对话。

## 创建 QA 配置文件

创建功能 PR 前，让 Copilot 创建可复用的 QA 配置文件，其中会定义 QA 执行的检查和必须遵守的边界：

1. 返回 Codespace，并确认筛选功能对话处于 Interactive 模式。
2. 请求默认智能体创建自定义智能体：

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## 检查配置文件

使用新智能体前，先审查其配置文件，确认 Copilot 已纳入预期的 QA 工作流和限制。这样可以避免智能体在只需验证功能时进行范围过大的修改。

1. 输入 `/diff` 并打开 `.github/agents/qa.agent.md`。
2. 阅读 frontmatter。`description` 是必填项；`name` 为可选项，但填写后可让智能体有清晰的显示名称。
3. 阅读配置文件指令，确认 QA 会从需求出发、遵循存储库指令、运行 `quality-checks` 技能并使用 Playwright MCP。
4. 确认 QA 会提供证据、修改实现代码前会先询问，而且不会提交更改或创建 PR。
5. 若配置文件遗漏任何职责或限制，请要求默认智能体修订后再继续。

## 根据议题运行 QA

Copilot CLI 会在对话开始时加载项目智能体。创建配置文件后恢复同一个筛选功能对话，再选择 QA，这样它就能使用已有的议题和规划决策。

1. 输入以下命令启用智能体：

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 刚创建的智能体可能尚未显示在可用列表中，但仍可使用上述命令启用。

2. 输入以下提示词，请 QA 智能体审查功能：

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. 等待 QA 智能体执行检查。
4. 阅读它生成的报告。

## 总结与下一步

你已在工作流中添加可复用的专业角色，并审查了它的工作。你了解了自定义智能体如何使用指令、技能和 MCP 工具，创建并检查 QA 配置文件，并根据筛选议题查看 QA 发现。

现在，代码实现、技能更新、QA 配置文件、测试和验证报告都已准备好供审查。下一步将[创建功能 PR 并使用 Agent Merge][next-lesson]。

## 资源

- [为 Copilot CLI 创建自定义智能体][create-agents]
- [自定义智能体配置][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
