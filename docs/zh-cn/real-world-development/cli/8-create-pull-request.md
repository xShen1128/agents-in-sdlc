---
title: "第 8 课 - 创建并合并功能 PR"
description: "一起审查筛选功能、指令、技能更新、QA 配置文件和测试，然后创建 PR 并使用 Agent Merge。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

筛选功能实现、指令更新、技能更新、质量保证 (QA) 配置文件和测试都保存在同一个分支中。现在要一起审查它们并创建拉取请求。之前你手动合并了星级评分 PR；这次将使用 **Agent Merge** 管理合并流程。

> [!NOTE]
> 正常情况下，功能、指令更新、技能更新和 QA 智能体会拆分成几个 PR。为了简化本工作坊，筛选和质量工作流都放在同一对话和分支中，并由这个 PR 一并提交。

本课将学习：

- 了解 Agent Merge 及其如何自动完成合并流程。
- 检查完整功能更改和验证证据。
- 创建筛选功能 PR。
- 审查后启用 Agent Merge，并确认 PR 已合并。

## 场景

在筛选功能工作流中，你使用 Copilot 规划、实现并验证了功能。Tailspin Toys 希望自动处理剩余 PR 工作，同时由开发人员保留合并授权。

## 了解 Agent Merge

**Agent Merge** 会自动完成合并拉取请求所需的剩余工作。启用后，Copilot 会处理 PR 的阻塞项，例如修复持续集成 (CI) 检查失败、响应审查评论以及在需要时变基；随后，在存储库允许的情况下启用 GitHub 自动合并。

之前你一直手动选择 **Merge pull request**。Agent Merge 可以负责此流程，但必须先审查工作并确认可以合并，然后再启用它。

## 使用 Agent Merge 管理 PR

代码已完成，接下来一起审查、创建 PR，并让 Agent Merge 管理剩余流程：

1. 返回 Codespace。
2. 输入 `/agent` 打开智能体对话框。
3. 从选项中选择 **Default**，然后按 <kbd>Enter</kbd>。
4. 输入 `/pr create` 创建 PR。
5. 输入 `/pr agentmerge` 启用 Agent Merge。
6. Copilot 会监控 PR 的持续集成流程。所有检查成功后，它会执行合并。
7. 确认看到 Copilot 显示类似“PR #14 was squash-merged successfully.”的消息。

> [!IMPORTANT]
> Agent Merge 不会绕过必需审批、分支保护、合并队列、存储库设置或权限要求。如果流程受阻，请阅读报告的原因；在存储库允许的情况下，完成审查后手动合并。

## 总结与下一步

你已使用 Copilot 生成代码、测试和验证代码，并自动化处理拉取请求流程。你了解了 Agent Merge，审查完整功能和验证证据，创建筛选功能 PR，并在审查后启用 Agent Merge 确认 PR 已合并。

接下来，[探索更多 Copilot CLI 斜杠命令][next-lesson]，了解上下文、模型、共享和可选的云智能体委派。

## 资源

- [使用 Copilot CLI 管理拉取请求][manage-prs]
- [Copilot CLI 命令参考][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
