---
title: "第 4 课 - 使用自定义指令引导 Copilot"
description: "查看存储库指令、添加文档标准，并将其应用于筛选功能代码。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

使用生成式 AI 时，上下文至关重要。如果任务必须按特定方式完成，就应确保 Copilot 能够获取相关指导。[指令文件][instruction-files]不仅说明要生成什么代码，还说明代码应如何组织。现在筛选功能已经实现，你将查看 Copilot 使用的指令、添加文档标准并将其应用于代码。

本课将学习：

- 存储库指令和按路径限定的指令文件如何传递给智能体。
- 如何更新指令文件以确保遵循编码标准。
- 如何观察指令文件对代码的影响。

## 场景

Tailspin Toys 为开发实践制定了一系列要求：

- 注释应说明目的和不明显的决策，而不是复述代码。
- `db/` 和 `src/lib/` 中的导出函数应通过 TSDoc/JSDoc 记录用途、参数和返回值；如果函数接受可注入的 `db` 参数，也应记录。
- 可复用 Astro 组件应记录其 `Props` 契约；相关代码更改时，注释也应保持更新。
- 保留现有格式和 lint 指导。

你将通过指令文件为 Copilot 提供这些要求。

## 指令文件

自定义指令为 Copilot 提供上下文和偏好设置，使它了解项目编码风格和要求。你可以指定编码规范、库以及希望添加的注释。指令可以适用于整个存储库，也可以针对特定文件类型或任务。

指令文件有两种类型：

- `.github/copilot-instructions.md`：针对存储库的每次请求都会发送此文件，应包含大多数请求所需的项目级信息。
- `.github/instructions/*.instructions.md`：为特定语言、文件类型或任务提供指导。

> [!NOTE]
> 不同环境对其他指令格式的支持有所不同。依赖特定格式前，请查看[自定义指令支持参考][custom-instructions-support]。

## 查看项目中的自定义指令文件

起始项目已包含一些指令文件。先查看现有内容，再进行修改，以便了解效果：

1. 返回 Codespace。
2. 在 Codespaces 编辑器（而非终端）中打开 `.github/copilot-instructions.md`。
3. 查看项目简介和编码指导。这些指令适用于此存储库中的每次 Copilot 交互。
4. 打开 `.github/instructions` 文件夹，查看 Astro、Drizzle 数据层、测试等指令。
5. 打开 `.github/instructions/unit-tests.instructions.md`，查看顶部的 `applyTo` 字段。它使用 glob 指定指令适用的文件。
6. 打开 `.github/instructions/drizzle.instructions.md`，查看它指向其他指令文件和项目文件的链接。通过这种方式，可以将较大的指令集拆分为可复用文件，并为 Copilot 提供可参考示例。

## 更新指令文件以符合团队指导

现有文件已是很好的起点，但还缺少一项标准。现在修改核心 `copilot-instructions.md`，要求新生成的 TypeScript 代码包含 TSDoc 注释：

1. 在 `.github/copilot-instructions.md` 中找到 **Code formatting guidance** 部分，通常在第 35 行附近。
2. 在该部分最后添加以下项目符号：

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

文件会自动保存。

## 使用更新后的指导

Copilot CLI 会在对话开始时加载存储库指令。保存指令后，退出并恢复筛选功能对话，这样 Copilot 会在保留功能上下文的同时加载新指导：

1. 如果筛选功能对话仍处于打开状态，输入 `/exit` 退出。Copilot CLI 会自动保存对话。
2. 在存储库根目录的 `game-filters-cli` 分支中恢复保存的对话：

   ```bash
   copilot --resume
   ```

   如有提示，选择筛选功能对话，并确认检出的是 `game-filters-cli` 分支。
3. 请求 Copilot 应用更新后的指导：

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. 输入 `/diff` 并审查更改的 TypeScript 文件。查看新增的 TSDoc 注释，并确认它们准确说明代码。

## 总结与下一步

你已了解 Copilot CLI 如何从指令文件获取上下文，并将新标准应用到功能代码中。接下来将[自定义并运行可复用的 quality-checks 技能][next-lesson]，确保 lint 和测试以一致方式运行。

## 资源

- [为 Copilot CLI 添加自定义指令][instruction-files]
- [自定义指令支持][custom-instructions-support]
- [创建自定义指令的最佳实践][instructions-best-practices]
- [Awesome Copilot — 指令文件集合及其他资源][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
