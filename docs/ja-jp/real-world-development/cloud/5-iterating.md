---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組んできました。Copilot にコードのドキュメントを追加させ、デザインチームが改善案を検討するための関連ゲーム機能を作成し、高コントラストとライトモードの切り替えを含むアクセシビリティ機能を実装しました。生成されたコードを確認し、必要に応じて Copilot にフィードバックを伝え、成果を改善しましょう。

### シナリオ

これまで何度も説明したように、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進めます。チームでのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は、監視なしで非同期にタスクを実行します。安全性を保つため、いくつかのセキュリティ制約が設けられています。

- Copilot はリポジトリに対する読み取り権限を持ち、書き込みは作業用ブランチに対してのみ許可されています。
- Cloud agent は GitHub Actions 内で動作し、作業用に独立した一時的な環境を作成します。
- GitHub Actions のワークフローは、人間が承認するまで実行されません。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した 1 つ目のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR 画面で作業します。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされたという警告が表示される場合があります。Copilot は外部 MCP サーバーへの接続を含め、既定では外部リソースへのアクセスが制限されているため、これは**想定どおり**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** または同様のタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合は、プルリクエストに **[WIP]** フラグが表示されます。その場合は、Copilot の作業が終わるまで待ちます。完了には数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更内容を確認します。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントなど、ドキュメントが追加されたコードを確認します。具体的な変更内容は異なる場合があります。

    変更を確認する際に、演習 2 の最初に確認したカスタム指示ファイルから適用された TSDoc コメント、TypeScript の規約、コメントヘッダーを探してください。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と協力する方法は、結果を一方的に受け取るだけではありません。チームメンバーと同じように、PR のコメントやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、修正に取り組む新しいセッションを開始します。結果は確率的に変わるため、依頼文を厳密に指定することはできません。Copilot への依頼例をいくつか示します。

- 各コードファイルの先頭に、役割を簡単に説明するコメントヘッダーを追加します。
- TypeScript ファイルと Astro ファイルに TSDoc のドキュメントコメントを追加します。
- Astro アプリの構成を説明する README を作成します。

1. 上記の例または自分で考えたドキュメントの改善内容を使い、**@copilot** にメンションして変更を依頼するコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot は更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが追加されています。
5. **Files changed** タブを選択して変更内容を確認します。

納得できるまで、Copilot と繰り返し作業できます。満足したら、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** または同様のタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下へスクロールします。
6. ワークフローが承認待ちであることを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace を開き、ターミナルで次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。ほかの作業と同様、Copilot cloud agent にコメントで更新を依頼して、反復して改善できます。たとえば表示件数を変更したり、今回のタスクはカスタム指示を更新する**前に**割り当てたため、コメントヘッダーと TSDoc のドキュメントコメントを追加するよう依頼したりできます。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼した高コントラストモードと、演習 4 でコントロールセンターから追加を依頼したライトモードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** または同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストに **[WIP]** フラグが表示されます。その場合は完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードを確認します。
5. 実装をレビューし、特に次の点に注意します。
   - モード切り替え用のトグル UI コンポーネント。
   - ユーザーの設定を保存する local storage の使用。
   - 高コントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認して問題がなければ **Conversation** タブに戻り、下へスクロールします。
7. ワークフローが承認待ちであることを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄にワークフローが追加されます。問題がなければ、単一 Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **省略可能:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace を開き、ターミナルで次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーで高コントラストとライトモードのトグルが動作すること、ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティのカスタムエージェントが、ベストプラクティスに沿った実装を Copilot に促したことを確認できます。アクセシビリティに懸念や改善点がある場合は、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 省略可能な演習 — 委任を続ける

Cloud agent に実際のバックログ項目を任せ、その間に別の作業を進めると効果的です。習慣づけとして、リポジトリにいくつか Issue を作成して Copilot に割り当ててみましょう。たとえば、次のような内容があります。

- ゲーム詳細ページに支援者向け関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認し、cloud agent の出力に反映される様子を把握しました。**
- **Copilot cloud agent に Issue を割り当て、環境の準備、計画、非同期の実行を確認しました。**
- **アクセシビリティ用のカスタムエージェントを作成して使用し、高コントラストとライトモードの切り替えを追加しました。**
- **Copilot agents ページをコントロールセンターとして使い、作業中のアクセシビリティセッションを監視して誘導しました。**
- **cloud agent のプルリクエストをレビューして反復改善し、`@copilot` に変更を依頼してワークフローを承認しました。**

## 振り返りと次のステップ

Cloud agent のハーネスを完了しました。さらに学習する場合は、ほかのハーネスで今回の内容を深められます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE で Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、Agent Skill、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` による cloud agent との連携を使い、ターミナルから 2 つの変更を確認してリリースします。

自分のリポジトリでは、次の方法を試してみてください。

- リファクタリングやテスト範囲の改善に関する Issue を cloud agent に割り当てます。
- 別の領域向けのカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot agents について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
