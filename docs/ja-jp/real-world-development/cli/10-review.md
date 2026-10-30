---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次の作業を行いました。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトと登録済みのフィルタリング Issue を確認した。
- 星評価を追加し、転送されたブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージした。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューした。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行した。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認した。
- 品質保証 (QA) カスタムエージェントを作成して選び、要件、カバレッジ、スキルの結果、ブラウザーでの確認証拠を評価した。
- フィルタリングの変更全体をレビューし、Agent Merge にフィルタリングの PR のマージを許可した。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認した。

## 作成したもの

このワークショップでは、更新済みの `main` からそれぞれ別のブランチを作成し、2 つの PR を作ります。

1. **星評価:** 既存の `starRating` と未評価の場合の表示をゲームカードに追加する。
2. **フィルタリングと品質確認:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して関連するテストを追加する。

フィルタリングの計画から PR の作成までは同じ会話とブランチを使いました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめています。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を使った Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果に要件とテストカバレッジのレビューをまとめました。

追加したテストは、実際の不足を解消するものでなければなりません。QA の結果、新しいテストが不要と判断される場合もあります。マージを許可する前に、コードと検証の証拠を確認してください。変更があった場合は、関連する検証結果も更新します。

## ベストプラクティス

Copilot に与えるコンテキストやツールは、生成結果に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を設定し、スキルは繰り返し実行できるタスクを記述し、カスタムエージェントは専門的な役割を定め、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の方針検討に、**Interactive** は対象を絞った変更を確認しながら進める場合に、**Autopilot** は範囲が明確なタスクを進める場合に使います。定型的な編集には高速なモデルを選び、複雑な作業にはより高性能なモデルを選びます。

インフラと同じくらい、コンテキストも重要です。何を、なぜ、どのように作りたいかを明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを終えました。Copilot CLI には、ほかにも次のような機能があります。

- `/review` でコードレビューエージェントに変更を分析させる。
- `/rubber-duck` で問題を相談し、別の視点を得る。
- `/fleet` で独立したサブタスクを並行して実行する。
- `/worktree` で別のタスクを分離する。
- `/delegate` で Copilot cloud agent にタスクを送信する。

## 次のステップ

ツールを上達させるには、使い続けることが一番です。本番コード、趣味のコード、いつか作ろうと思っていた小さなアプリなどで活用してみましょう。チームと学びを共有し、互いの経験から学んでください。ドキュメントも引き続き確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のカスタマイズ][customize-cli]
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
