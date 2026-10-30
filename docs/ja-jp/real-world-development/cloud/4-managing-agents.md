---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、Copilot cloud agent にユーザー体験の改善と機能追加に関する 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクを管理する方法はいくつかあります。[GitHub.com の agents ページ][agents-page]もその 1 つです。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を出したりできます。

このレッスンでは、次の内容を学習します。

- agents ページから cloud agent のタスクを監視する。
- 進行中のセッションに追加の機能を依頼する。

## シナリオ

ハイコントラストモードの作成をエージェントに依頼した後、チームはライトモードも追加するとよいと考えました。サイトのスタイル更新と切り替え機能にすでに取り組んでいるため、同時に追加するのが合理的です。ハイコントラストに加えてライトモードも実装されるよう、エージェントに指示します。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状況を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左ペインのタスクリストを確認します。次のような、Copilot に割り当てたタスクが表示されます。
   - コードのドキュメントを更新する。
   - ゲーム変更用の API を生成する。
   - Web サイトにハイコントラストモードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリのコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP サーバーを設定する。
   - 割り当てられたタスクを完了するための作業を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状況や進め方によって異なります。

4. 右側に表示される PR ペインも確認します。ここから PR と変更ファイルを確認できます。

## cloud agent に追加の指示を出す

実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. ハイコントラストモードのセッションを選択します。正確なタイトルは、Copilot が付けた名前や作業状況によって異なります。

   ![mission control に表示されるアクセシビリティのセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まるまで数分間セッションを確認します。作業開始後は、次の例のようなメッセージが表示されます。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

   ```plaintext
   While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
   ```

   ![agents ページの Copilot cloud agent タスクで、作業中のセッションに指示するダイアログが示されたスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に反映したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取りかかります。メッセージを送信した時点で実行していた手順が終わると、新しい依頼を作業の流れに取り込みます。

前と同じように完了まで数分かかります。その間に休憩して、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

GitHub Copilot cloud agent のタスクを監視して方向付ける中心的な場所である、Copilot agents ページについて学びました。mission control から、進行中と完了済みのタスクを追跡し、実行中の作業をレビューし、進行中のタスクの範囲やガイダンスを変更できます。

このレッスンでは、次の内容を学習しました。

- mission control と agents ページから cloud agent のタスクを監視した。
- 進行中のセッションに追加機能を依頼した。

Copilot がアクセシビリティ機能を完成させる間に、次は[作成されたプルリクエストを改善します][next-lesson]。

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
