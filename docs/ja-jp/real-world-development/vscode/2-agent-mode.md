---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は関連するファイルを探して変更し、テストを実行し、デバッグし、変更漏れのファイルを見つけて、さらに修正します。作業は続きます。

そこで役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業できるよう設計されています。開発者と同様に、既存のプロジェクト構造を調査し、必要な変更を行い、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェントモードでサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けです。エージェントモードは IDE 内で作業するため、すばやくフィードバックを得てやり取りできます。Cloud agent はチームのメンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェントモードでプロジェクトを調査し、関連ファイルを特定して、複数の変更を連携させる方法。
- GitHub Copilot エージェントモードで UI とデータ層をまたいで新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリと発行元で絞り込めるようにしたいと考えています。前の演習で発行元ヘルパーを追加したので、Copilot エージェントモードを使って、データ層、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認し、現在の機能を理解しましょう。

Web サイトは開発者向けテーマのボードゲームを扱うクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示します。サイトは単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。各ページは frontmatter で Drizzle ORM を通じてローカルの SQLite データベースを直接クエリします。独立したバックエンド API やクライアントサイドの UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな機能は標準 DOM API を使う小規模で範囲を限定した Astro `<script>` に追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. Codespace に戻ります。前の演習の後であれば `main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルウィンドウを開きます。
3. `main` を更新し、フィルター機能用の新しいブランチを作成します。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備が完了すると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くか尋ねるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> Codespace で Codespace ターミナルの localhost URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーを実行している Codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認します。主な機能は次のとおりです。

- **ホームページ**: ボードゲームのタイトル、画像、説明の一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの詳しい情報が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を開始します。モードとモデルの選択は演習ごとに異なるため、必要な場合は各手順で説明します。

Web サイトの初期実装は機能しますが、さらに機能を追加したいと考えています。まずバックログを確認します。テンプレートからプロジェクトをセットアップすると、Issue のバックログが自動で作成されています。GitHub Copilot にその一覧を表示してもらいます。

1. Chat ビューのエージェント選択欄で **Agent** を選択します。**Agent** は自律的に変更を計画・実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。このワークショップでは Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードを比較的一貫させ、学習しやすくすることを意図しています。ただし、LLM は確率的に動作するため、ワークショップに記載された内容と異なる提案が表示される場合があります。これは正常な動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot が異なる MCP コマンドを使うことがありますが、タスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信し、Issue のバックログについて尋ねます。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot が、ユーザーに最も役立つと判断した項目を優先順位付けしていることがわかります。

## 指示ファイルを確認する

エージェントにコード生成を依頼する前に、Copilot にコンテキストを渡すための指示ファイルを確認します。Web サイトへの機能追加方法を説明する [ユーザーインターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. Codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加の進め方に関するガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要。
   - コンポーネント設計、テスト容易性、アクセシビリティの原則。
   - 次のファイル種別に関する指示へのリンク。
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルとプロジェクト内のファイルを参照できます。パスは指示ファイルの場所を基準とします。これにより、指示を再利用可能な小さな単位に分け、複雑さを抑え、例やテンプレートを示せます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの更新が必要です。

- データ層 (`src/lib/`) に残りのフィルターロジックを追加または調整します。
- フィルター動作のテストを追加または更新します。
- ゲーム一覧ページにフィルター UI を追加します。

さらに、コードベースにマージする前にテストを実行し、すべて成功することを確認します。Copilot エージェントモードにこれらの作業を依頼しましょう。

1. 現在の Copilot の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント選択欄で **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調査し、必要なファイルを探す様子を確認します。データ層のヘルパー、UI、テストを特定してファイルを編集し、テストを実行します。

   ![Copilot がプロジェクトファイルを調査する画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など複数の作業を行います。タスクとコードベースの複雑さによって、数分かかることがあります。その間にエディターで **Keep** と **Undo** が表示される場合があります。Copilot が作業中の変更を確定または取り消す必要はありません。完了後にすべての変更について **Keep** または **Undo** を選択できます。

7. Copilot に求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテストの実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業の途中で処理が一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、タスクを完了してエラーがないと判断するまで作業します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot やほかの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新しい機能を確認します。
11. すべてが動作することを確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。次のことを確認しました。

- GitHub Copilot エージェントモードで UI とデータ層にまたがる機能を実装できます。
- Copilot エージェントモードでプロジェクトを調査し、関連ファイルを特定して、複数の変更を連携できます。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューできます。

次は [Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを作成します。

### ボーナスの演習 — ページ分割を実装する

ゲームの一覧が増えたら、ページ分割が必要になります。この演習で学んだことを活用して、Copilot にページ分割を実装するよう依頼します。次の点を検討してください。

- 指示ファイルを使うなど、既存のベストプラクティスに従います。
- ユーザーがページサイズを選べるようにするか、固定値にするかなど、実装方法を決めます。
- 希望する実装に必要なガイダンスを、プロンプトに含めます。
- 必要に応じて GitHub Copilot と反復し、変更を依頼してコンテキストを追加します。Copilot を使う際の通常の進め方です。

## リソース

- [Copilot の Ask、Edit、Agent モード: それぞれの用途][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
