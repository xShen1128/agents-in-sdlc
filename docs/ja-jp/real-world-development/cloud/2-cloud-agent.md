---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残っている機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能追加などの作業を自律的に実行するよう設計されています。作業が完了すると、人間がレビューできるドラフト PR を作成します。これにより、手間のかかる作業を任せて開発プロセスを加速し、開発者はより大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成の環境をカスタマイズします。
- 操作を安全に行う方法を確認します。
- 明確に範囲を定めた Issue の重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した委託業者は、ドキュメントを十分に整備していませんでした。まず、アプリケーション内のすべてのエクスポート関数に TSDoc のドキュメントコメントを追加したいと考えています。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリのほかのゲームを表示して、閲覧を続けられるようにします。まだ完成度の高い実装は必要ありません。UX の受け入れテストに使えるものを求めています。この作業は現在のブロッカーですが、より優先度の高い Issue もあります。

どちらも優先順位が下がりやすい作業であり、Copilot cloud agent に割り当てるのに適しています。cloud agent に非同期で作業を任せれば、開発者は別の作業に集中し、後から戻って成果をレビューできます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じように、バックグラウンドで作業を実行できます。人間の開発者に作業を依頼する方法が複数あるように、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、いくつかの方法があります。割り当てると Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境を準備して作業を開始します。作業中または完了後に Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な作業でも、詳しく見ると複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を明確にしてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学び、試しながら調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習では、カスタム指示と Copilot のガイド方法を学びました。カスタム指示の実習を行ったことがあれば、`.github/copilot-instructions.md` の効果を確認済みです。まだの場合は、今が内容を確認するよい機会です。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれている指示ファイルを読み取り専用で確認し、後でその影響を見られるようにします。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクションを確認します。特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認してください。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターで、指示の対象が `**/*.test.ts` ファイルに限定されていることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成されたプルリクエストに TSDoc コメント、TypeScript の規約、コメントヘッダーが含まれるか確認してください。これらは指示ファイルから適用されます。後の演習で PR をレビューする際にも確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は特定の環境と、正常に作業できる状態にするセットアップスクリプトが必要です。Copilot にタスクを割り当てる場合も同様で、Copilot はソフトウェアエンジニアと同じように作業します。

Cloud agent は GitHub Actions を使って作業環境を用意します。作業を始める前に実行する[専用のセットアップワークフロー][setup-workflow]を作成して、環境をカスタマイズできます。このワークフローは `.github/workflows/copilot-setup-steps.yml` に構成します。必要な開発ツールや依存関係を利用できるようにします。演習の流れを簡潔にし、学習に集中できるよう、ラボではすでに設定済みです。Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリに加え、単一 Astro アプリの SQLite データベースをマイグレーションして初期データを投入します。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Copilot には操作用の専用トークンが付与されます。セットアップ手順に必要な最小限の権限を設定します。
    permissions:
      # たとえば依存関係をインストールするため、セットアップ手順内でリポジトリをクローンする場合は contents: read が必要です。クローンしない場合は、手順の完了後に Copilot が自動でクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は現在 Astro + Drizzle/libSQL です)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカルの SQLite データベースをマイグレーションして初期データを投入します。ビルドとテストでデータが必要です。
      - name: Set up the database
        run: npm run db:setup
```

ほかの GitHub Actions ワークフローと同様に見えますが、重要な点がいくつかあります。

- `copilot-setup-steps` という単一のジョブが含まれています。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たずに、ワークフローが正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、情報が古い、または不足しているプロジェクトは少なくありません。このような技術的負債は放置されがちで、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、この Issue を Copilot cloud agent に割り当てるのが適しています。バックグラウンドで必要なドキュメントを作成してもらい、後の演習で成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue の作成画面を開きます。
4. **Blank issue** を選択して新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当て画面を開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前です。一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成したことを確認できます。

> [!NOTE]
> Copilot が PR に todo リストを表示するまで、数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を決めています。

13. リストと、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下へスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションを表示するボタン](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された状態が表示されるまで、ページの再読み込みが必要な場合があります。

16. 進行中のセッションをスクロールして、Copilot の作業を確認します。コードを調査して状態を理解し、計画を検討して判断した後、コードを作成する様子がわかります。

この作業には数分かかることがあります。Copilot cloud agent の主な利点は、非同期に作業を任せ、開発者がほかの作業に集中できることです。この特徴を活かし、もう 1 つのタスクを cloud agent に割り当ててから、アプリケーションに機能を追加する作業に取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザインチーム向けに関連ゲームを追加する作業に時間がかからないとしても、その間に別の作業ができます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して Issue の作成画面を開きます。
4. 空のテンプレートを使うため、**Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して割り当て画面を開きます。

    ![Copilot を Issue に割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことがわかります。

![目の絵文字で Issue に取り組んでいることを示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組み始めました。Copilot cloud agent はソフトウェアエンジニアと同様に作業するため、常に監視する必要はありません。完了後にレビューできます。次は、カスタムエージェントを作成して使用します。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]を使いました。cloud agent に Issue を割り当て、非同期で実行できます。技術的負債への対応、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などに活用できます。

このレッスンでは、次のことを行いました。

- コード生成用の環境をカスタマイズしました。
- 操作を安全に行う方法を確認しました。
- 範囲を明確にした Issue の重要性を学びました。
- Issue を Copilot に割り当てました。

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
