---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を活用するうえで、コンテキストは重要です。作業の進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に「何を」してほしいかだけでなく、「どのように」作業してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを使いません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムに並べて効果を示すのが難しいからです。このハーネスの後半で cloud agent が作成したプルリクエストをレビューするときに、追加した指示ファイルの影響を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。例として、次のような項目があります。

- データ層には常にユニットテストが必要です。
- UI はダークモードにし、現代的な印象にします。
- TSDoc のドキュメントコメントを使ってコードを説明します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらの開発慣行に沿って作業するために必要な情報を伝えます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと好みを Copilot に伝えられます。好みの規約やライブラリ、コードに含めたいコメントなどを指定して、より適切な提案やコードスニペットを得るために活用できます。リポジトリ全体、または特定のファイル種別やタスク向けの指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中の内容の概要、ベストプラクティスなど、Copilot Chat や CLI での多くのリクエストに関係するプロジェクト全体の情報を記述します。
- 特定の言語 (TypeScript など)、ファイル種別、UI コンポーネントの作成やテストの追加などのタスク向けには、`.github/instructions/*.instructions.md` を作成できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの規格の指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成に関する詳細はこの演習の対象外ですが、サンプルプロジェクトには代表的な構成例があります。基本的な方針は次のとおりです。

- `copilot-instructions.md` には、開発するプロジェクトや構造、全般的なコーディング標準など、プロジェクト全体に関係する情報を記述します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データ層など、特定のファイル種別やタスクに関する指示に使います。
- 自然な言葉で、明確なガイダンスと例を記述します。

指示ファイルの使い方は 1 つに限られません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意すると役立ちます。このプロジェクトの指示ファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があることに気付くでしょう。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で操作は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`)。関連するレッスンで手順を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めたリポジトリ [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。共通の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加えて、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやり取りで役立ちます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示を適用するファイルの glob を指定しています。この例では、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成するときに適用される指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイル先頭のコメントヘッダーに関するルールを追加します。

## リポジトリ全体の標準を追加する

この演習で行う編集は 1 つです。コード内のドキュメントは TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーで記述する、というプロジェクト全体のルールを追加します。cloud agent は、このハーネスの後半で割り当てる Issue に取り組む際に、このルールを参照します。

編集する前に作業用ブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、ここでは変更を整理してコミットします。

1. Codespace のターミナルで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

先ほど確認したとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。リポジトリのコーディング標準を更新し、コード提案の質を高めましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準を説明していますが、コード内ドキュメントのルールはまだありません。そのため、生成されたヘルパー関数にドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に、次の Markdown を追加して、Copilot にファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、言語、ファイル種別、タスクに応じた `*.instructions.md` として作成することもできます。ドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記述します。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み取ります。次の演習で Copilot に Issue を割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を使えるように、変更を `main` に取り込む必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で Issue を割り当てたときに、cloud agent がこの指示を読み取れるようになります。

> [!TIP]
> このワークショップでは、ブランチを使わずに `main` で直接作業してもかまいません。ここでブランチを使うのは、実際のプロジェクトでの進め方を体験するためです。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを得る仕組みを確認し、リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加しました。このハーネスで後から cloud agent が作成するプルリクエストをレビューするときに、追加した標準を確認できます。

次に、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
