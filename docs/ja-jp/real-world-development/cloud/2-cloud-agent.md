---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ課題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot cloud agent は、コードの更新や機能の追加といった作業を自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフト PR を作成します。面倒な作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent では、次の内容を学習します。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確に定める重要性を理解する。
- Issue を Copilot に割り当てる。

## シナリオ

Tailspin Toys には解消したい技術的負債があります。最初のサイトを作成した委託先がドキュメントを適切に整備しておらず、実際にはほとんどありません。まず、アプリケーションのエクスポートされたすべての関数に TSDoc の doc comment を追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリーの別のゲームを関連ゲームとして表示し、閲覧を続けられるようにします。今は完成度の高い実装ではなく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、優先度がより高い Issue もあります。

どちらも優先順位が下がりやすい作業の例であり、Copilot cloud agent に任せるのに適しています。cloud agent が非同期に作業する間、開発者は別のタスクに集中し、後から戻って作業をレビューできます。

## GitHub Copilot cloud agent を使う

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法があります。割り当てると、Copilot は進捗を追跡するドラフトプルリクエストを作成し、環境を準備してタスクに取りかかります。作業中でも完了後でも、Copilot のセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot は魔法ではありません。考える作業を省略できるわけではありません。一見簡単に見える操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めましょう。AI ペアプログラマーとして扱い、段階を踏んで学び、試し、調整します。生成 AI を加えても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot への影響を確認しました。すでにカスタム指示のハンズオンを完了していれば `.github/copilot-instructions.md` を実際に見たことがあるでしょう。まだの場合は、ここで簡単に確認できます。Copilot cloud agent に作業を割り当てる前に、このリポジトリに用意された指示ファイルを読み、後でその影響を確認できるようにしましょう。

リポジトリの GitHub Web UI、または起動済みの Codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc の doc comment に関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter を確認します。この指示は `**/*.test.ts` に適用されます。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、生成された PR にある TSDoc の doc comment、TypeScript の規約、コメントヘッダーに注目してください。これらは指示ファイルによるものです。後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent の開発環境を設定する

コードを作成するには、通常、適切な環境と初期化スクリプトが必要です。タスクを Copilot に割り当てる場合も同様で、Copilot はソフトウェアエンジニアのように作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を利用できるように環境をカスタマイズできます。このワークショップでは、学習を進めやすいように事前設定済みです。単一の Astro アプリのため、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションとシードが済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの Actions タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 自分の手順に必要な最小限の権限を設定します。Copilot の操作には専用トークンが使われます。
    permissions:
      # セットアップでリポジトリをクローンする場合 (たとえば依存関係のインストール) は `contents: read` が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体は Astro と Drizzle/libSQL)
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

      # ローカルの SQLite データベースをマイグレーションし、シードデータを投入します
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、次の点に注目してください。

- `copilot-setup-steps` という単一のジョブがあります。このジョブは Copilot が PR の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、Copilot の実行を待たずにリポジトリの **Actions** タブからワークフローを手動実行し、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、情報が古かったり、まったくなかったりするプロジェクトは少なくありません。こうした技術的負債は放置されやすく、生産性を下げ、コードベースの保守や新しい開発者の参加を難しくします。Copilot はドキュメントの作成を得意とするため、cloud agent に任せるのに適した Issue です。バックグラウンドで必要なドキュメントを作成してもらい、後の演習でレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. `[WIP]: Code lacks documentation` のようなタイトルの新しい PR を開きます。一覧にまだ表示されない場合は、しばらく待ってからブラウザーを更新します。
12. 数分後、Copilot が todo リストを作成したことを確認します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は環境を準備し (前述のワークフローを実行)、プロジェクトを分析して、作業の進め方を決めています。

13. todo リストと、実行予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションを表示する画面](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新された表示を確認するため、ブラウザーウィンドウを更新する必要がある場合があります。

16. ライブセッションをスクロールして Copilot の作業を確認します。コードを調査して状態を把握し、計画を考えて判断した後、コードを作成する様子が分かります。

完了まで数分かかる場合があります。Copilot cloud agent の主な利点は、作業を非同期に実行し、その間に別の作業ができることです。2 つ目のタスクを cloud agent に割り当て、その間にアプリケーションへ機能を追加しましょう。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分けられることです。デザインチーム向けの関連ゲーム機能は時間のかからない作業かもしれませんが、その時間を別の作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を開始したことを示す 👀 が Issue の最初のコメントに表示されます。

![Copilot が Issue の作業中であることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように作業するため、常に監視する必要はなく、完了後にレビューできます。次はカスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期に作業を任せられます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに活用できます。

このレッスンでは、次の内容を学習しました。

- コード生成環境をカスタマイズする。
- 操作を安全に行う。
- Issue の範囲を明確に定める重要性を理解する。
- Issue を Copilot に割り当てる。

cloud agent がバックグラウンドで作業する間に、次はカスタムエージェントを作成して使います。[Copilot cloud agent は MCP サーバー][cloud-agent-mcp]も使用でき、前のモジュールで確認したカスタム指示も利用できます。

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
