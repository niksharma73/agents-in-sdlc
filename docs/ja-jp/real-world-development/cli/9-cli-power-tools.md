---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウドへの委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI には対話に使えるスラッシュコマンドが多数あります。高度な機能、内部情報、追加の設定オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などのコマンドを使いました。ここでは、ほかの便利なコマンドを確認します。

このレッスンでは、次の内容を学習します。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認する。
- `/model` で、利用可能なモデルを確認する。
- `/share` でセッションをエクスポートまたは共有する方法を学ぶ。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認する。

## シナリオ

CLI の主要なワークフローが終わりました。ここでは、コンテキスト管理、モデルの切り替え、セッション共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを調べる

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。また、スラッシュコマンドを使って確認したり、自分で圧縮したりできます。

1. Codespace に戻り、Copilot CLI が起動していない場合はリポジトリルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域の内訳を確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合は、変化が小さいことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。関係のない別タスクに移るときに新しい会話を始めるには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルによって得意なことが異なり、開発者ごとに好みも異なります。Copilot CLI では、利用可能なモデルを確認して選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用できるモデルと使用状況の情報を確認します。
3. 現在のモデルを使い続けるか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学んだことを共有すると、AI ツールの活用方法を互いに高められます。`/share` は、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist に公開したりできます。

1. `/help` を入力し、インストール済みのバージョンで利用できる `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合った出力先を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. 送信または公開する前に、エクスポート内容を確認します。セッションの出力にはプロンプト、応答、プロジェクトの詳細が含まれることがあります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容を公開しないでください。

## 省略可能: 作業を拡張または委任する

ワークショップの主要な内容は完了です。Copilot CLI には、大きなタスクに役立つコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` は、別のタスク向けに分離された Git worktree を作成できます。
- `/delegate` は、タスクを Copilot cloud agent に送信できます。cloud agent は非同期に作業し、プルリクエストを作成する場合があります。

これらのコマンドを使うと、追加の worktree やリモート作業が生じる可能性があるため、任意の演習です。試すときは、範囲を明確に定めた新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期エージェントの作業を詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次の内容を学習しました。

- `/context` と `/compact` で、Copilot が会話のコンテキストを管理する方法を確認した。
- `/model` で、利用可能なモデルを確認した。
- `/share` でセッションをエクスポートまたは共有する方法を学んだ。
- 並行作業、worktree、cloud agent への委任に使う任意のコマンドを確認した。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。次は[学んだ内容を振り返り][next-lesson]、さらに学ぶための手順を確認しましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
