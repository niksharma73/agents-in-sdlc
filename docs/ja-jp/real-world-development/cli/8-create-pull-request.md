---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて 1 つのブランチに保存されています。まとめてレビューしてプルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしました。今回は **Agent Merge** にマージの作業を任せます。

> [!NOTE]
> 通常は機能、指示、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは手順を簡潔にするため、フィルタリングと品質確認の作業を 1 つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次の内容を学習します。

- Agent Merge と、マージまでの工程を自動化する仕組みを理解する。
- 機能全体の変更と検証の証拠を調べる。
- フィルタリングの PR を作成する。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認する。

## シナリオ

フィルタリング機能の作業を通じて、Copilot で機能を計画、実装、検証しました。Tailspin Toys では、開発者がマージを許可する権限を保ったまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げている問題 (失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要に応じたリベースなど) に取り組みます。その後、リポジトリで許可されていれば GitHub の自動マージを有効にします。

これまでは **Merge pull request** を自分で選択していました。Agent Merge にこの作業を任せることもできますが、有効にする前に内容をレビューして、マージしてよいか判断してください。

## Agent Merge で PR を管理する

すべてのコードを作成したら、まとめてレビューして PR を作成し、残りの工程を Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェント選択画面を開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. `PR #14 was squash-merged successfully.` のような Copilot のメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。処理がブロックされた場合は報告された理由を確認し、リポジトリで許可されていればレビュー後に手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの工程など、開発作業をいくつか自動化しました。このレッスンでは、次の内容を学習しました。

- Agent Merge と、マージまでの工程を自動化する仕組みを理解した。
- 機能全体の変更と検証の証拠を調べた。
- フィルタリングの PR を作成した。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認した。

次は、コンテキスト、モデル、共有、任意のクラウドへの委任に役立つ[スラッシュコマンドを確認します][next-lesson]。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
