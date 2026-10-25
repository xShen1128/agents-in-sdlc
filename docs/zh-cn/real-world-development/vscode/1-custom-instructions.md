---
title: "练习 1 - 自定义指令（VS Code）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：智能体模式 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按特定方式完成，或者 Copilot 需要了解背景信息，就应确保它能够获取这些信息。[指令文件][instruction-files]是提供代码生成指导的重要方式。

本练习将介绍：

- 如何通过存储库自定义指令和按路径限定的指令文件，为 Copilot 提供项目上下文、编码指南和文档标准。
- 如何使用现有指令生成筛选功能的第一部分数据层代码（发行商助手）。
- 如何在 `.github/copilot-instructions.md` 中添加一项全局标准。
- 如何重新发送相同提示词，观察生成代码如何采用新标准。
- 如何将指令更新和筛选功能基础提交到 `main`，供下一课中的 Copilot 使用。

> [!CAUTION]
> 生成的代码可能不完全符合你设定的标准。Copilot 具有非确定性。本练习的目标是观察更新指令前后行为的总体变化，而非逐字符匹配输出。

## 指令文件

### 场景

Tailspin Toys 为开发实践制定了一系列要求：

- 数据层必须有单元测试。
- UI 应采用暗色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头应有一段注释，说明文件的用途。

你将使用指令文件为 Copilot 提供这些要求。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好设置，使它能够理解编码风格和要求。你可以指定编码规范、库以及注释要求。指令可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件有两种类型：

- `.github/copilot-instructions.md`：针对存储库的每次请求都会发送此文件，应包含项目级信息，例如技术栈、项目概况、最佳实践和全局指导。
- `.github/instructions/*.instructions.md`：可用于特定文件类型或任务，例如 TypeScript、Astro、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体会在生成代码时使用存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

完整说明请参阅[自定义指令支持参考][custom-instructions-support]。

### 管理指令文件的最佳实践

示例项目展示了一种典型做法：

- 在 `copilot-instructions.md` 中集中记录项目级指导，例如项目用途、目录结构和全局编码标准。
- 使用 `*.instructions.md` 为特定文件类型（如单元测试、Astro 组件和数据层）或任务提供指导。
- 使用自然语言，保持要求清晰，并提供正反示例。

创建指令文件没有唯一正确的方法。通过试验找出适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的指令文件。此项目为不同任务提供了指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以帮助生成指令文件。不同产品入口的操作有所不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，或 Copilot CLI 中的 `/init`。当前学习路径会在相关位置说明操作方式。
>
> 如需模板或起点，可浏览包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot]。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

先查看存储库提供的指令文件：其中有一个核心 `copilot-instructions.md`，还有一组针对不同任务的 `*.instructions.md` 文件。可以在编辑器或 GitHub Web 界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 下还有 **GitHub Actions Workflows** 指导，这些内容适用于与 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。它使用相对于存储库根目录的 glob，匹配适用文件，例如 `**/*.test.ts`。
5. 阅读其中有关创建单元测试的要求。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部链接。指令文件可拆分为较小、可复用的文件，也可以链接到项目示例。这里的路径相对于指令文件所在位置。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 介绍了项目编码标准，但尚未要求添加代码文档。接下来将添加 TSDoc 注释和文件说明注释的规则。

## 创建更改分支

按照最佳实践，为更改创建分支：

1. 返回上一课的 codespace。
2. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
3. 创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions
   ```

## 更新指令前先使用 Copilot Chat

> [!TIP]
> **打开 Copilot Chat**
>
> 开始下面的练习前，返回 codespace 并在 Copilot Chat 面板中选择 **New Chat**，开始新的对话。模式和模型因练习而异，具体要求会在相应步骤中说明。

先使用当前指令文件发送提示词，之后更新指令并重新发送相同提示词，比较差异：

1. 关闭上一个练习留下的编辑器标签页，避免 Copilot 获取不需要的上下文。
2. 打开 `src/lib/publishers.ts`，让 Copilot 知道助手应放在哪里。
3. 在 Chat 视图的智能体下拉菜单中选择 **Agent**，以便 Copilot 修改文件。

   ![Chat 视图中智能体选择器的截图](../../../_images/shared-chat-mode-selector.png)

4. 发送以下提示词：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot 会探索项目并应用代码更新，通常会同时修改助手文件和测试。
6. 查看生成的助手：它应是一个接受 `db` 客户端并返回类型化发行商数组的函数。这来自 `.github/instructions/drizzle.instructions.md` 中适用于 `src/lib/*.ts` 的数据层约定。
7. 注意生成代码中**缺少** TSDoc 文档注释和文件级说明注释。

> [!CAUTION]
> Copilot 具有非确定性，即使没有要求，也可能添加文档注释。如果出现这种情况也没关系；本练习的重点仍是观察更新指令后的一致性变化。

## 添加新的存储库标准

`.github/copilot-instructions.md` 用于向 Copilot 提供项目级信息。现在添加一项编码标准，改善生成的代码建议：

1. 重新打开 `.github/copilot-instructions.md`。
2. 找到 **Code formatting requirements** 部分，通常在第 27 行附近。这里描述了项目编码标准，但尚未规定代码文档要求。
3. 在现有标准下方添加以下 Markdown，要求 Copilot 添加文件头注释和 TSDoc 文档注释：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存 `copilot-instructions.md`。

> [!TIP]
> 指令文件可以放在存储库级别（`.github/copilot-instructions.md`）提供全局指导，也可以使用 `*.instructions.md` 针对特定语言、文件类型或任务。刚添加的规则适用于整个项目，因此应放在存储库级文件中。

## 重新发送提示词并观察变化

1. 返回 Copilot Chat，选择 **New Chat** 清空上下文。
2. 再次打开 `src/lib/publishers.ts`，让 Copilot 聚焦到正确文件。
3. 发送与之前**相同的提示词**：

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 查看新文件开头是否出现类似以下注释块：

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 查看函数是否包含类似以下 TSDoc 注释：

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

你已引导 Copilot 遵循新的项目标准，并将其应用到下一课要继续使用的实际代码中。

## 提交指令并推送分支

指令文件与存储库中的其他内容一样，应通过源代码管理进行维护。提交并推送分支：

1. 按 <kbd>Control</kbd>+<kbd>`</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>`</kbd>（Windows/Linux）打开新终端。
2. 运行以下命令，确认指令更新和助手更改均已存在：

   ```bash
   git status
   ```

3. 暂存并提交指令更新，以及 Copilot 为助手基础功能修改的所有文件：

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # 如果 git status 显示还有其他相关更改（例如测试），也请将这些文件添加到暂存区。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 推送分支：

   ```bash
   git push -u origin custom-instructions
   ```

## 创建并合并拉取请求

分支推送后，创建拉取请求并关联待办列表中的文档标准议题。也可以让 Copilot 使用已连接的 GitHub 工具查找议题、创建并合并 PR。

> [!NOTE]
> 这是第一次让 Copilot 代表你操作 GitHub。项目模板已连接 GitHub 工具，无需额外设置。Copilot 首次使用工具时，VS Code 可能提示你登录 GitHub；按提示允许操作。稍后的练习会介绍此连接的工作方式（模型上下文协议）。

1. 在 codespace 中打开 Copilot Chat。
2. 按 <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd>（Mac）或 <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd>（Windows/Linux）打开 Copilot Chat 视图，并确认智能体选择器中选中 **Agent**。
3. 请求 Copilot 查找更新指令文件的议题，并从当前分支创建 PR，说明指令更新和发行商助手基础工作：

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot 会查找议题并创建 PR。
5. 如果 Copilot 提示允许它代表你执行 GitHub 操作，请先检查命令，再酌情选择 **Allow**。
6. PR 创建后，使用以下提示词请求 Copilot 将 PR 合并到 `main` 并更新本地分支：

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. 如果 Copilot 提示允许 GitHub 操作或 Shell 命令，请先检查命令，再酌情选择 **Allow**。

你已在 GitHub Copilot 的帮助下创建并合并了拉取请求。

## 总结与下一步

你了解了 Copilot 如何从项目指令文件获取上下文，并在 VS Code 的 Copilot Chat 中使用现有指令发送代码生成提示词、添加存储库级标准、重新发送相同提示词以观察效果，以及将指令更新和助手基础功能提交到 `main`。

接下来使用[智能体模式][next-lesson]，让 Copilot 跨代码库添加新功能。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [GitHub Copilot 个人自定义指令][personal-instructions]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
