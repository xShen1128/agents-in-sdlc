---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で Issue に基づくフィルター機能を実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初に、小さな機能をプロジェクトに追加しました。より大きな変更には、さらにしっかりとした手順が必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作するよう設計されているため、適切な方法で適切な機能を構築できます。このレッスンから複数回にわたり、エージェントを活用した一般的な開発プロセスを実践します。Issue から機能を実装し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトへマージします。

> [!NOTE]
> 機能の開発を続ける間は、同じ会話とブランチを使います。通常はファイル種別ごとにブランチや PR を分けますが、ここでは基本概念に集中するため簡略化します。

このレッスンでは、次のことを行います。

- GitHub Issue から Copilot との新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新しい機能を実装します。
- コードをレビューします。
- 転送したブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加して、QA エージェントを作成してから機能 PR を作成します。

## シナリオ

Tailspin Toys のカタログが増えており、訪問者がカテゴリや発行元でゲームを絞り込めるようにする必要があります。バックログの Issue に機能の概要は記載されていますが、カテゴリを組み合わせる方法など、実装前に合意しておく詳細もあります。Plan モードでこれらを決めてから、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発フローに導入しても、基本は変わりません。むしろ、その重要性は増します。多くの開発者は、次のような流れで作業します。

1. 実装する内容を記述した Issue から始めます。
2. 実装方針を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新しい機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、コードをマージします。

> [!NOTE]
> 詳細な手順はチームや組織によって異なりますが、多くの場合は上記の流れを基本とします。

この標準的な進め方を守ることで、AI が生成したコードが要件を満たし、手書きのコードと同じ確認プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどれだけ自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントが変更を提案し、次に進む前にユーザーの入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを記述し、テストを実行し、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルター機能の作業を始める前に、Codespace に戻ってリポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動してください。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルター機能用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの Issue を選択します。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub の Issue やプルリクエスト (PR) をコンテキストに追加できます。

## フィルター機能を計画する

計画を立てると、Copilot に実装やタスクを任せる前に、機能の方針を決められます。複雑な作業では、時間をかけて計画することをお勧めします。Plan モードに切り替えて、計画を作成するよう Copilot に依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 直前の手順で追加した Issue への参照に続けて、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査し、最適な実装方法を検討して計画を作成します。

3. フィルター機能の動作について質問されたら、希望に沿って回答します。正解は 1 つではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画全体を確認します。
6. 判断と合わない部分があれば、修正を依頼します。

## Autopilot を承認する

計画を作成して確認したら、実装に進みます。Autopilot モードを使って Copilot に作業を任せましょう。

Autopilot を使うと、Copilot はタスクが完了したと判断するまで問題に取り組みます。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンで同様の名前の項目を選択します。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

コードが生成されたら、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** または **Autopilot** ではないことを確認します。
2. `/diff` を実行し、フィルター機能の実装とテストを確認します。
3. 結果を Issue および計画時の決定事項と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードはよさそうですが、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. カテゴリ、発行元、計画で合意した組み合わせを使って絞り込みます。
4. リセットと検索結果がない場合の動作が、Issue と決定事項に沿っていることを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

会話モードを切り替えながら機能を実装してレビューしました。このレッスンでは、次のことを行いました。

- GitHub Issue から Copilot との新しい会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送したブラウザーで機能を手動検証しました。

次は、コード生成が文書化された慣行に従うよう、[カスタム指示を使います][next-lesson]。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
