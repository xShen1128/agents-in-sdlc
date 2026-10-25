---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を活用するうえで、コンテキストは重要です。タスクの進め方に決まりがある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。特に重要なのが[指示ファイル][instruction-files]です。コード生成に関するガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認します。
- 現在の指示を使って、フィルター用の最初のデータスライス (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、新しい標準に沿ってコードが生成される様子を確認します。
- 次の演習で Copilot が更新済みのガイダンスを利用できるよう、指示の更新とフィルター機能の最初の実装を `main` にコミットします。

> [!CAUTION]
> 生成されたコードは、設定した標準の一部に従わない場合があります。Copilot は確率的に動作します。この演習では、出力が一字一句一致することではなく、指示を更新した後の傾向の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。例として次のような項目があります。

- データ層には常にユニットテストが必要です。
- UI はダークモードにし、現代的な印象にします。
- TSDoc のドキュメントコメントを使ってコードを説明します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるよう必要な情報を伝えます。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと好みを Copilot に伝えられます。好みのコーディング規約やライブラリ、コードに含めたいコメントなどを指定し、より適切な提案やコードスニペットを得るために活用できます。リポジトリ全体、または特定のファイル種別やタスクに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中の内容、ベストプラクティスなど、Copilot への多くのリクエストに関係するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、TypeScript や Astro などの特定言語、ファイル種別、UI コンポーネントやユニットテストの作成などのタスクに対する指示を記述します。

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
- 自然な言葉で明確なガイダンスを記述し、例を示します。

指示ファイルの使い方は 1 つに限られません。試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意すると役立ちます。このプロジェクトの指示ファイルを確認すると、[UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示があることに気付くでしょう。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で操作は異なります (たとえば VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init`)。関連するレッスンで手順を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースを集めた [awesome-copilot][awesome-copilot]を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

リポジトリには共通の `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルが用意されています。エディターまたは GitHub の Web UI で開いて確認します。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明に加え、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやり取りで役立ちます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどに関する指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートを基準とする glob で対象ファイルを指定します。この例では、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトでユニットテストを作成するときに適用される指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開いて末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示を小さく再利用可能なファイルに分け、例を参照できるようにします。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションではプロジェクトのコーディング標準を説明していますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメントコメントとファイル先頭のコメントヘッダーを追加するルールを設定します。

## 変更用のブランチを作成する

ベストプラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使用した Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を開始します。モードとモデルの選択は演習ごとに異なるため、必要な場合は各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送ります。後で指示を更新し、同じプロンプトをもう一度送って違いを確認します。

1. Copilot が必要なコンテキストだけを使うよう、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーの作成場所を Copilot に示します。
3. Chat ビューのエージェント選択欄で **Agent** を選び、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを更新します。ヘルパーファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されたヘルパーは、`db` クライアントを最初の引数として受け取る型付き関数であり、発行元の型付き配列を返すことを確認します。これは、`src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約によるものです。
7. 生成されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後に一貫性が向上することが、この演習の目的です。

## リポジトリ全体に適用する標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案の質を高めるため、リポジトリのコーディング標準を記述しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準を説明していますが、コード内ドキュメントのルールはまだありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準のすぐ下に次の Markdown を追加し、ファイルコメントヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、言語、ファイル種別、タスクに応じた `*.instructions.md` として作成することもできます。ドキュメントコメントのルールのようなプロジェクト全体の標準は、リポジトリレベルのファイルに記述します。

## 同じプロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を再び選択し、Copilot に対象ファイルを示します。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で利用する実際のコードに適用できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもリポジトリ内のほかのファイルと同様、ソース管理で管理します。変更をコミットして、リポジトリにブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新とヘルパーの基盤として Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加ファイルが表示される場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、バックログのドキュメント標準に関する Issue と関連付けたプルリクエストを作成します。手動でも作成できますが、接続済みの GitHub ツールを使って Copilot に任せましょう。Copilot に Issue を探して PR を作成させ、さらにマージを依頼します。

> [!NOTE]
> この演習では、初めて Copilot に GitHub での操作を依頼します。プロジェクトのテンプレートでは GitHub のツールがワークスペースに接続済みです。初めてツールを使うときに VS Code から GitHub へのサインインを求められた場合は、画面の指示に従って許可してください。この接続の仕組みである Model Context Protocol については、後の演習で学びます。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択欄で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する Issue を見つけ、現在のブランチから PR を作成するよう Copilot に依頼します。PR には指示の更新と発行元ヘルパーの基盤の両方を記載します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue の検索と PR の作成を開始します。
5. Copilot が代理で GitHub 操作を行うことを **Allow** で求められたら、コマンドを確認し、適切な場合に許可します。
6. PR が作成されたら、Copilot にマージを依頼し、ローカルのブランチを `main` に戻します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が代理で GitHub 操作とシェルコマンドを実行することを **Allow** で求められたら、コマンドを確認し、適切な場合に許可します。

GitHub Copilot のサポートを受けて、プルリクエストを作成してマージできました。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを得る仕組みを確認し、VS Code の Copilot Chat を使って次のことを行いました。

- コード生成のプロンプトを送り、既存の指示で Copilot が生成する内容を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、新しい標準に沿ってコードが生成される様子を確認しました。
- 次の演習で基盤として使えるよう、指示の更新と発行元ヘルパーを `main` にコミットしました。

次は、コードベース全体に新機能を追加する [エージェントモード][next-lesson]で指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
