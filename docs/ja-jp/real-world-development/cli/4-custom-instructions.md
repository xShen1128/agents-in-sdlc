---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルター機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を活用するうえで、コンテキストは重要です。タスクの進め方に決まりがある場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、求めるコードだけでなく、その構成方法も伝えられます。フィルター機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる仕組みを確認します。
- コーディング標準を守るために指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プロセスに関するガイドラインと要件があります。例として次のような項目があります。

- コメントにはコードの言い換えではなく、意図や自明でない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。該当する場合は注入可能な `db` 引数も説明します。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連コードを変更した場合はコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使い、Copilot がこれらの開発慣行に沿って作業できるよう必要な情報を伝えます。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストと好みを Copilot に伝えられます。好みのコーディング規約やライブラリ、コードに含めたいコメントなどを指定し、より適切な提案やコードスニペットを得るために活用できます。リポジトリ全体、または特定のファイル種別やタスクに対する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対するすべてのリクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクトレベルの情報を記述します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイダンスを記述します。

> [!NOTE]
> ほかの形式の指示への対応状況は、利用環境によって異なります。特定の形式を使う前に、[カスタム指示のサポート][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、あらかじめいくつかの指示ファイルが含まれています。変更を加える前に内容を確認し、影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。これらの指示は、このリポジトリで Copilot とやり取りするすべての場合に適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro ファイル、Drizzle データ層、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを指定する glob です。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存ファイルへのリンクを確認します。これにより、指示を小さく再利用可能なファイルに分け、参考にする例を Copilot に示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。中核となる `copilot-instructions.md` を更新して、新しく生成する TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを探します。35 行目付近にあります。
2. 次の Markdown を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。機能のコンテキストを保ったまま新しいガイダンスを読み込むため、指示を保存した後でフィルター機能の会話を終了し、再開します。

1. フィルター機能の会話がまだ開いている場合は、`/exit` を入力して閉じます。Copilot CLI は会話を自動保存します。
2. `game-filters-cli` ブランチにいることを確認し、リポジトリのルートで保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合はフィルター機能の会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に、更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を実行して、変更された TypeScript ファイルを確認します。新しく追加された TSDoc コメントがコードを正確に説明していることを確かめます。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパス別の指示ファイルがエージェントに伝わる仕組みを確認しました。
- コーディング標準を守るために指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次に、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
