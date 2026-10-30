---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードでフィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認しましょう。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーで機能をテストしてもらいます。その後、ブランチを公開して PR を開きます。

この演習では、次の内容を学習します。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot のツールを拡張する方法を学ぶ。
- Playwright MCP サーバーをワークスペースに追加する。
- Copilot にブラウザーでフィルタリング機能を手動テストさせる。
- ブランチを公開して、フィルタリングのプルリクエストを開く。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にアクセスできると、さらに便利になります。Copilot は Model Context Protocol (MCP) を使って外部ツールやサービスと通信します。これはエージェントが外部のツールやサービスとやり取りするための標準的な方法です。

![エージェントモードがコンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールと連携する仕組みを示した図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) は、AI エージェントが外部のツールやサービスと通信するための仕組みです。MCP を使うと、エージェントはリソースを通じて最新情報にアクセスし、ツールを使ってユーザーに代わり操作できます。

ツールやリソースには、AI エージェントと外部サービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカルツールとエージェントとの通信を管理します。各 MCP サーバーは、エージェントが利用できるツールとリソースのセットを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。新しいリポジトリの作成、既存のリポジトリの更新、Issue やプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright によるブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

さまざまなツールやリソースを使える MCP サーバーがほかにもあります。GitHub は、エコシステム内のサーバーを見つけたり貢献したりしやすくするため、[MCP レジストリ](https://github.com/mcp)をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクト内のほかの依存関係と同様に扱ってください。使用前にソースコードをよく確認し、発行元を検証し、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使用し、機密性の高いリソースや操作へのアクセス許可には注意してください。

## MCP の設定を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. Codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに設定されていることを確認します。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       }
     }
   }
   ```

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot が Issue のバックログを読み取れました。[GitHub リモート MCP サーバー][remote-github-mcp-server]を使用するため、ローカルにインストールする必要はありません。VS Code は HTTP で接続し、Copilot が最初にツールを使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できるブラウザーを提供します。機能をテストするために使います。

1. `.vscode/mcp.json` で `github` と並べて `playwright` エントリを追加し、次のようにします。

   ```json
   {
     "servers": {
       "github": {
         "type": "http",
         "url": "https://api.githubcopilot.com/mcp/"
       },
       "playwright": {
         "command": "npx",
         "args": ["@playwright/mcp@latest", "--headless"]
       }
     }
   }
   ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグは、Codespace のようにブラウザーを表示するデスクトップ環境がない場所で Playwright を実行できるようにします。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストに Playwright を使うため、必要なブラウザーはすでにインストールされている場合が多いです。後で Copilot がブラウザーがないと報告した場合は、`npx playwright install chromium` を実行して再度試すよう依頼します。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて使うとき、VS Code はサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` の `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認して信頼を選択し、サーバーを起動します。

手動で起動する必要はありません。省略した場合は、次の手順で Copilot がツールを初めて使うときにサーバーが起動し、信頼の確認が表示されます。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]は、Copilot が操作できる実際のブラウザーを提供します。自分でサイトを操作しなくても、エージェントがページを開き、移動し、フィルターを適用して結果を読み取り、観察内容を要約できます。会話を離れずに機能の動作を確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティツリー][playwright-mcp-server]を使います。つまり、エージェントはボタン、リンク、リスト項目などの構造化されたラベル付き要素を使って推論します。これは支援技術と同じ方法なので、簡単な機能チェックがアクセシビリティの基本確認にもなります。

サーバーを接続し、アプリを実行した状態で、Copilot に次の手順を試すよう依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、各手順を実行して結果を報告します。Issue の受け入れ条件に照らして報告を読みます。問題がある場合は、追加の質問をするか、プルリクエストを開く前に修正を依頼します。

> [!NOTE]
> テストするには、アプリを `http://localhost:4321` で実行しておく必要があります。開発サーバーを停止した場合は、プロンプトを送る前にもう一度起動します。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーをダウンロードする場合があります。ブラウザーがないと報告されたら、`npx playwright install chromium` を実行して再度試すよう依頼します。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームでレビューするためにプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. Codespace の **Source Control** パネルに移動し、Copilot が行った変更を確認します。
2. **+** アイコンを選択して変更を stage します。
3. **Sparkle** ボタンを使ってコミットメッセージを生成します。

   ![Copilot の変更が表示された Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリに push します。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot に PR を作成してもらいましょう。関連する Issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを始めます。
2. GitHub ツールを使用できるよう、エージェント選択欄で **Agent** が選択されていることを確認します。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 情報の取得や操作の実行に必要な場合は **Continue** を選択します。GitHub ツールを初めて使うときに GitHub で認証を求められる場合があります。画面の指示に従って許可します。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成したことを確認します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューします。**まだマージしないでください。**

## まとめと次のステップ

この演習では、次の内容を学習しました。

- Model Context Protocol (MCP) とは何か、MCP サーバーで Copilot のツールを拡張する方法を学んだ。
- Playwright MCP サーバーをワークスペースに追加した。
- ブラウザーでフィルタリング機能を手動テストしてからリリースする方法を確認した。
- ブランチを公開し、フィルタリングのプルリクエストを作成した。

機能のテストに MCP サーバーを使いましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は[カスタムエージェントを作成し][next-lesson]、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
