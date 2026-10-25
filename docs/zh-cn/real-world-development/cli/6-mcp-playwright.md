---
title: "第 6 课 - 使用 Playwright MCP 验证功能"
description: "在 Copilot CLI 中查看或配置 Playwright MCP，并在浏览器中探索筛选功能。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

编写代码只是开发工作的一部分。我们还需要处理数据和外部服务，并为 Copilot 提供额外的自动化能力。MCP 服务器可以让 Copilot 使用 CLI 内置功能以外的工具和服务。

本课将学习：

- 了解模型上下文协议 (MCP) 及 Copilot CLI 如何使用它。
- 如果尚未配置，则添加 Playwright MCP 服务器。
- 让智能体操作浏览器并探索筛选功能。

## 场景

单元测试和端到端测试很重要，但验证 UI 更新还需要实际与 UI 交互。希望 Copilot 像用户一样使用网站，进一步自动化更改检查，并提高对功能行为符合预期的信心。

## 什么是模型上下文协议 (MCP)？

[模型上下文协议 (MCP)][mcp-blog-post]为 AI 智能体与外部工具、服务通信提供了一种方式。通过 MCP，智能体可以实时访问最新信息并代表你执行操作。

智能体通过 MCP 服务器访问工具和资源。MCP 服务器充当 AI 智能体与外部工具、服务之间的桥梁；每个服务器都提供一组可供智能体使用的工具和资源。

常见的 MCP 服务器包括：

- **[GitHub MCP Server][github-mcp]**：提供管理 GitHub 存储库、议题和拉取请求的 API。
- **[Playwright MCP Server][playwright-mcp-server]**：使用 Playwright 提供浏览器自动化功能。

还有许多其他 MCP 服务器。GitHub 提供了 [MCP 注册表][mcp-registry]，便于发现和参与生态系统项目。

> [!CAUTION]
> 应像对待项目的其他依赖项一样看待 MCP 服务器。使用之前，请审查源代码、验证发布者并评估安全影响。

## 添加 Playwright MCP 服务器

如果尚未配置 Playwright MCP 服务器，请先添加它，以便 Copilot 像用户一样与网站交互。

> [!NOTE]
> 如果 Copilot CLI 已配置 Playwright MCP，请跳过这些设置步骤并继续下一节。

1. 返回 Codespace。
2. 在 Copilot CLI 中输入以下命令，打开添加 MCP 服务器对话框：

   ```plaintext
   /mcp add
   ```

3. 输入名称 `playwright`，然后按 <kbd>Tab</kbd>。
4. 确认服务器类型为 **STDIO**，按 <kbd>Enter</kbd>，再按 <kbd>Tab</kbd>。
5. 在 **Command** 对话框中粘贴：

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. 按 <kbd>Control</kbd>+<kbd>S</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>S</kbd>（Windows/Linux）保存新的 MCP 服务器。
7. 按 <kbd>Esc</kbd> 退出 MCP 对话框。

## 让 Copilot 使用 Playwright 探索功能

之前你已手动确认功能正常。现在使用 Copilot 中配置的 Playwright MCP 服务器重复检查：

1. 输入以下提示词，让 Copilot 使用 Playwright MCP 验证功能：

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常不必明确要求 Copilot 使用 MCP 服务器，因为它会自行判断。不过指出应该使用的工具，有助于获得更一致的结果，也能节省一些 token。

2. 观察 Copilot 如何列出浏览器中的操作步骤并检查功能。
3. 阅读报告，确认行为符合预期。

Copilot 会启动服务器、使用 Playwright 与网站交互、停止服务器，并提供报告。

## 总结与下一步

你已在 Copilot CLI 中使用 Playwright MCP 服务器通过真实浏览器探索功能。你了解了 MCP，按需添加了 Playwright MCP，并让智能体检查筛选功能。

接下来，[创建 QA 自定义智能体][next-lesson]，让技能和浏览器工具在专门的角色中协同工作。

## 资源

- [MCP 是什么，为什么大家都在讨论？][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [向 Copilot CLI 添加 MCP 服务器][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
