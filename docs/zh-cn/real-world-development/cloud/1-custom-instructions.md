---
title: "练习 1 - 自定义指令（云智能体）"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 上一课：先决条件][previous-lesson] · [下一课：Copilot 云智能体 →][next-lesson]

使用生成式 AI 时，上下文至关重要。如果任务必须按特定方式完成，或 Copilot 需要了解背景信息，就应确保它能够获取这些信息。[指令文件][instruction-files]可提供相关指导，让 Copilot 不仅了解要做什么，也了解应该如何完成。

本练习将介绍：

- 如何通过存储库自定义指令和按路径限定的指令文件，为 Copilot 提供项目上下文、编码规范和文档标准。
- 如何在 `.github/copilot-instructions.md` 中添加一项新的全局标准。

> [!NOTE]
> 与 VS Code 和 CLI 学习路径不同，本练习不会比较提示词的前后效果。Copilot 云智能体会异步处理 GitHub 议题，因此难以实时并排展示指令的影响。稍后审查云智能体创建的拉取请求时，可以看到指令文件产生的影响。

## 指令文件

### 场景

Tailspin Toys 为开发实践制定了一系列要求：

- 数据层必须有单元测试。
- UI 应采用暗色模式并具有现代感。
- 代码应包含 TSDoc 文档注释。
- 每个文件开头应有一段注释，说明文件的用途。

通过指令文件，可以确保 Copilot 了解这些实践要求并据此完成任务。

### 自定义指令

自定义指令为 Copilot 提供上下文和偏好设置，使它能够更好地理解编码风格和要求。你可以指定编码规范、库以及希望添加的注释类型。指令既可适用于整个存储库，也可针对特定文件类型或任务。

指令文件有两种类型：

- `.github/copilot-instructions.md`：每次针对该存储库的请求都会发送此文件。它应说明项目级信息，例如技术栈、项目概况、最佳实践，以及 Copilot Chat 或 CLI 请求通常都需要的背景。
- `.github/instructions/*.instructions.md`：可用于特定文件类型或任务，例如 TypeScript、UI 组件或单元测试。

> [!NOTE]
> 在 IDE 中，指令文件仅用于 Copilot Chat 生成代码，不用于代码补全或下一步编辑建议。
>
> Copilot Chat、Copilot CLI 和 Copilot 云智能体会在生成代码时使用存储库级指令，以及带有 `applyTo` frontmatter 的 `*.instructions.md` 文件。
>
> Copilot 还支持其他标准的指令文件，例如 AGENTS.md 和 CLAUDE.md。

更多背景信息请参阅[自定义指令支持][custom-instructions-support]。

### 管理指令文件的最佳实践

完整讨论如何创建指令文件超出了本工作坊的范围。示例项目展示了一种典型做法：

- 在 `copilot-instructions.md` 中集中记录项目级指导，例如项目用途、目录结构和全局编码标准。
- 使用 `*.instructions.md` 为文件类型（如单元测试、Astro 组件和数据层）或特定任务提供详细指导。
- 使用自然语言，保持指导清晰，并提供正反示例。

创建指令文件没有唯一正确的方法。通过试验，找出适合项目的做法。

> [!TIP]
> 每个使用 GitHub Copilot 的项目都应有一组完善的指令文件。项目中有多种任务对应的指令，例如 [UI 更新][ui-instructions]和 [Astro][astro-instructions]。
>
> Copilot 也可以协助生成指令文件。不同产品入口的操作不同，例如 VS Code 中的 **Configure Chat → Generate Agent Instructions**，以及 Copilot CLI 中的 `/init`。当前学习路径会在相关位置说明具体操作。
>
> 如需模板或起点，可浏览包含指令文件、自定义智能体和其他资源的 [awesome-copilot][awesome-copilot] 存储库。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## 查看项目中的自定义指令文件

先查看此存储库提供的指令文件：其中有一个核心 `copilot-instructions.md`，还有一组针对不同任务的 `*.instructions.md` 文件。可在编辑器或 GitHub Web 界面中打开。

1. 打开 `.github/copilot-instructions.md`。
2. 查看项目简介，以及 **Agent notes**、**Code standards**、**Scripts** 和 **Repository Structure** 等部分。**Code standards** 中还有一节 **GitHub Actions Workflows**，这些指导适用于 Copilot 的各种交互。
3. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等相关指令。
4. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。它使用相对于存储库根目录的 glob；例如 `**/*.test.ts` 会匹配 TypeScript 测试文件。
5. 查看其中有关创建单元测试的项目要求。
6. 打开 `.github/instructions/drizzle.instructions.md` 并查看底部的链接。指令文件可以拆分为更小、可复用的文件，并链接到项目示例。这里的路径相对于指令文件所在位置，而不是存储库根目录。

> [!NOTE]
> `copilot-instructions.md` 中的 **Code formatting requirements** 记录了项目编码标准，但尚未要求添加代码文档。接下来将添加 TSDoc 注释和文件说明注释的要求。

## 添加新的存储库标准

接下来要添加的唯一编辑，是要求代码中包含 TSDoc 文档注释和文件级说明注释。之后，云智能体会在本学习路径中处理你分配的议题时使用这些要求。

先创建一个分支。云智能体会读取议题目标分支中的指令，但此处仍应整洁地提交更改：

1. 在 codespace 终端中创建并切换到新分支：

   ```bash
   git checkout -b custom-instructions-cloud
   ```

2. 重新打开 `.github/copilot-instructions.md`，找到 **Code formatting requirements** 部分。
3. 在现有规范下方添加以下 Markdown：

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. 保存文件。

> [!TIP]
> 指令文件可以放在存储库级别（`.github/copilot-instructions.md`），提供全局指导；也可以使用 `*.instructions.md` 文件为特定语言、文件类型或任务提供指导。刚添加的文档注释标准适用于整个项目，因此应放在存储库级文件中。

## 提交、推送并合并指令更新

云智能体会从议题目标分支读取指令。下一课将把议题分配给 Copilot，届时它会从 `main` 创建分支。因此，必须先将指令更改合并到 `main`，云智能体才能使用它们。

1. 暂存并提交更改：

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. 推送分支：

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. 在 github.com 上创建从 `custom-instructions-cloud` 合并到 `main` 的拉取请求，并将其合并。下一课分配的议题将使用这些指令。

> [!TIP]
> 如果希望在本工作坊中直接使用 `main`，可以跳过分支并直接提交。创建分支是为了模拟真实项目中的工作方式。

## 总结与下一步

你已了解 Copilot 如何从项目指令文件获取上下文，并在 `.github/copilot-instructions.md` 中添加了一项全局标准。之后可以在云智能体创建的拉取请求中观察这项标准的效果。

接下来，[将第一个议题分配给 Copilot 云智能体][next-lesson]。

## 资源

- [GitHub Copilot 自定义指令文件][instruction-files]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [编写更好的 Copilot 自定义指令的 5 个技巧][copilot-instructions-five-tips]
- [Awesome Copilot — 指令文件和其他资源集合][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
