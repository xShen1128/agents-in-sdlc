---
title: "第 5 课 - 自定义并使用 quality-checks 技能"
description: "查看现有的 quality-checks 技能、自定义报告格式，并使用它验证筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

编写代码只是开发工作的一部分。我们已手动验证代码，并通过指令文件确保代码遵循标准。接下来还需要考虑测试、lint 和持续集成 (CI) 的其他环节。

这些任务适合使用**智能体技能**。技能可以指导 Copilot 如何正确地执行这些操作。

本课将学习：

- 查看现有的 `quality-checks` 技能。
- 自定义结果报告格式。
- 重新加载并运行技能。

## 场景

Tailspin Toys 使用 `quality-checks` 技能运行单元测试、lint 和类型检查。团队希望改进报告，让结果更易于阅读。

## 指令、脚本和资源

智能体技能会封装可复用的任务指令、可执行脚本和辅助资源，智能体可以按需加载。技能本质上是包含 `SKILL.md` Markdown 文件的文件夹。该 Markdown 文件包含说明技能名称和用途的 frontmatter、技能功能概述，以及何时调用技能的指导。文件夹还可以包含供技能使用的脚本和其他资源。

> [!NOTE]
> 技能不一定要包含额外文件夹或文件。Tailspin Toys 的 `quality-checks` 技能只包含 `SKILL.md`，因为它使用项目已有命令。

技能可以放在项目的 `.github/skills` 文件夹中，作为团队共享和复用的存储库资源；也可以放在用户技能目录 `~/.copilot/skills` 中。

## 查看技能

查看 Tailspin Toys 团队创建的 `quality-checks` 技能，其中包含运行单元测试、lint 和类型检查的步骤：

1. 返回 Codespace，在 Codespaces 编辑器中打开 `.github/skills/quality-checks/SKILL.md`。
2. 阅读顶部的 `name` 和 `description`。描述有助于 Copilot 判断何时调用技能。
3. 阅读指令，了解技能如何引导 Copilot 执行测试和 lint。
4. 注意技能目前还没有 **Results output formatting** 部分。

## 修改前先运行技能

可以直接通过 Copilot CLI 调用技能，也可以使用自然语言。现在请求 Copilot 运行三项检查：

1. 返回筛选功能对话，并确认处于 Interactive 模式。
2. 输入以下提示词：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 阅读技能最后生成的报告。

## 自定义报告

希望报告说明运行了哪些检查、是否成功，以及工具报告的实际结果。现在更新技能，让它生成这样的报告：

1. 重新打开 `.github/skills/quality-checks/SKILL.md`。
2. 在文件末尾添加以下部分：

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. 文件会自动保存。

## 运行更新后的技能

更改完成后，Copilot CLI 无需重启对话即可重新加载编辑后的技能：

1. 输入：

   ```plaintext
   /skills reload
   ```

2. 使用与之前**完全相同的提示词**：

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 阅读新报告，并与第一次报告比较。

## 总结与下一步

你已自定义并使用现有智能体技能，查看了用于单元测试、lint 和类型检查的 `quality-checks` 技能，自定义了结果格式，然后重新加载并运行技能。

这项更改将与筛选功能一起进入功能 PR。接下来让 Copilot 通过 [Playwright MCP 服务器][next-lesson]直接与网站交互。

## 更多技能示例

以下社区示例仅供参考，不是本练习的额外任务：

- [Agent Skills 规范][skill-spec]
- [贡献工作流：`make-repo-contribution`][contribution-example]
- [需求文档：`prd`][prd-example]
- [图表和内置导出脚本：`drawio`][drawio-example]
- [浏览器测试：`webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
