---
title: "レッスン 7 - QA エージェントの作成と使用"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザーからフィルタリングを確認しました。次は、明確な QA プロセスを持つカスタムエージェントにこれらの機能をまとめます。

このレッスンでは、次の内容を学習します。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認する。
- 再利用可能な品質保証プロファイルを作成して調べる。
- QA エージェントを選び、フィルタリングの Issue に照らして結果をレビューする。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫してレビューしたいと考えています。カスタムエージェントを使うと、この QA プロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門的な役割です。プロファイルにはエージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

ここまで使ったカスタマイズには、それぞれ別の役割があります。リポジトリの指示はチームの標準を説明します。`quality-checks` スキルは繰り返し実行できるチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、要件を評価して結果を報告するために Copilot がこれらをどう使うかを示します。これらの代替ではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルを Copilot に作成してもらいます。プロファイルでは、QA が実施するチェックと、その作業の境界の両方を定めます。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA ワークフローと境界が設定されているか確認します。これにより、検証だけを行うつもりなのにエージェントが機能を変更する、といった事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、設定するとエージェントを見分けやすくなります。
3. 指示を読み、QA が要件を確認し、リポジトリの指示に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が根拠を示して報告し、実装コードを変更する前に確認を求め、変更をコミットせずプルリクエストも開かないことを確認します。
5. 役割や境界が不足している場合は、既定のエージェントに修正を依頼してから次に進みます。

## Issue を QA で確認する

Copilot CLI は会話の開始時にプロジェクトのエージェントを読み込みます。プロファイルの作成後、同じフィルタリングの会話を再開し、QA を選択します。そうすると、会話に含まれる Issue と計画の決定事項を活用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。次のコマンドで有効化できます。

2. 次のプロンプトで QA エージェントに機能をレビューさせます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を始めます。
4. 作業が完了したら、レポートを確認します。

## まとめと次のステップ

再利用可能な専門エージェントを追加し、その作業をレビューしました。このレッスンでは、次の内容を学習しました。

- カスタムエージェントが指示、スキル、MCP ツールと連携する仕組みを確認した。
- 再利用可能な品質保証プロファイルを作成して調べた。
- QA エージェントを選び、フィルタリングの Issue に照らして結果をレビューした。

実装、スキルの更新、QA プロファイル、テスト、検証レポートの準備ができました。次は[機能の PR にまとめ、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用カスタムエージェントを作成する][create-agents]
- [カスタムエージェントの設定][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
