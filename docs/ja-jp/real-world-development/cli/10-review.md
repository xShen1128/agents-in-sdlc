---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、参考リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。具体的には、次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルター機能の Issue を探しました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルター機能の Issue から始め、Plan モードで方針を決め、Autopilot モードで実装して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) server を追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの確認結果を評価しました。
- フィルター機能全体をレビューし、その PR で Agent Merge を承認しました。
- コンテキスト、モデル、共有、任意のクラウド委任に使うスラッシュコマンドを確認しました。

## 作成したもの

ワークショップでは、更新済みの `main` から別々のブランチを作成し、2 つの PR を作成しました。

1. **星評価:** 既存の `starRating` と、評価がない場合の明示的な表示をゲームカードに追加します。
2. **フィルター機能と品質のワークフロー:** フィルター機能を実装し、指示を更新して機能に適用します。`quality-checks` のレポート形式をカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルター機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを簡潔にするため、それらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルでは、ユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は、これらの結果を要件とテスト範囲の確認に加えて評価しました。

追加するテストは、実際の不足を解消するものであるべきです。QA の実行で新しいテストが不要と判断されても、問題ありません。マージを承認する前に、コードと検証の証拠を確認してください。変更後は、影響する検証を再実行してください。

## ベストプラクティス

Copilot に与えるコンテキストやツールは、作業内容に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを構成しました。こうしたカスタマイズは会話を越えて再利用し、チームのニーズに応じて調整できます。指示は標準を定め、スキルは繰り返し可能なタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて **モードとモデル**を選びます。構築前に方針を検討するには **Plan**、範囲を絞った変更を確認しながら進めるには **Interactive**、範囲が明確なタスクを自律的に進めるには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業には高性能なモデルを選びます。

開発環境と同様に、コンテキストも重要です。何を、なぜ、どのように実装するかを明確に説明すると、結果が大きく変わります。

## さらに試す

基本のワークフローを学びました。次の CLI 機能も試してみてください。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を相談し、別の視点から考えます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` でタスクを Copilot cloud agent に送ります。

## 次のステップ

どのツールも、使い続けることで活用方法が上達します。本番コードや個人のコードで、あるいは長い間作ろうと思っていた小さなアプリで試してください。チームのメンバーと学んだことを共有し、互いの知識を取り入れましょう。いつでもドキュメントを確認してください。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進んでください。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
