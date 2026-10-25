---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

直前の 2 つの演習では、ユーザー体験の改善と機能追加を目的とする 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。このコントロールセンターでは、未完了のプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行された操作を確認したり、進行中のセッションに指示を送ったりできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

エージェントに高コントラストモードを作成するよう割り当てた後、チームはライトモードも追加したいと考えました。サイトのスタイルを更新し、モードを切り替えられるようにする作業がすでに進んでいるため、ライトモードも含めるのが合理的です。高コントラストモードに加えてライトモードも実装するよう、エージェントの作業を誘導します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メインペインと左ペインに表示されるタスクの一覧を確認します。Copilot に割り当てた次のタスクが表示されるはずです。
    - コードベースのドキュメントを更新する。
    - ゲームを変更するための API を生成する。
    - Web サイトに高コントラストモードを追加する。
3. 実行中のタスクを 1 つ選択します。Copilot が実施した次のような作業を確認できます。
    - リポジトリのコードをチェックアウトする。
    - Copilot が作業する環境を作成する。
    - MCP サーバーを設定する。
    - 割り当てられたタスクを完了するためにさまざまな手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や採用したアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更ファイルを表示し、進捗を監視できます。

## cloud agent を誘導する

実行中のタスクを確認したので、高コントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. 高コントラストモードのタスクに対応するセッションを選択します。タイトルは Copilot が付けた名前と作業状況によって異なります。

    ![コントロールセンターに表示されたアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を開始したことがわかるまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページのクラウドエージェントタスクで、実行中セッションに指示するダイアログを示す画面](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に反映する様子を確認します。

## Copilot に作業を続けてもらう

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを受け取った時点で実行中の手順が完了した後、新しい依頼を作業に取り入れます。

今回も完了まで数分かかるため、これまでに学んだことを振り返りながら待ちます。

## まとめと次のステップ

Copilot cloud agent のタスクを監視してガイドするための中心的な場所である Copilot agents ページについて学びました。このコントロールセンターでは、進行中および完了済みのタスクを追跡し、作業内容を確認して、進行中のタスクに指示を追加できます。

このレッスンでは、次のことを行いました。

- コントロールセンターとエージェントページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、次は [Copilot が作成したプルリクエストを反復して改善します][next-lesson]。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
