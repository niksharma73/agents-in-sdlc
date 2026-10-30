---
title: "演習 5 - GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このハンズオンでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot といくつかのタスクに取り組みました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要であれば Copilot にフィードバックを送って改善しましょう。

### シナリオ

これまでに何度も確認したとおり、生成 AI を加えてもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを進める必要があります。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## GitHub Copilot cloud agent とセキュリティ

Copilot cloud agent は非同期に、監督者なしで作業するため、安全性を保つためのセキュリティ制約があります。

- Copilot はリポジトリを読み取れますが、書き込みできるのは作業用のブランチ**のみ**です。
- Cloud agent は GitHub Actions 内で動作し、作業用に分離された一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間の承認が必要です。
- [外部リソースへのアクセスは既定で制限されており](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)、MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールが GitHub Copilot をブロックしたという警告が表示される場合があります。MCP サーバーへの外部呼び出しを含む外部リソースへのアクセスは既定で制限されているため、これは**想定された動作**です。必要であれば、[Copilot cloud agent のファイアウォール][agent-firewall]をカスタマイズまたは無効化できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** などのタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。その場合は作業が完了するまで待ちます。数分かかることがあるため、休憩したり、学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更内容をレビューします。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc の doc comment など、更新されたコードとドキュメントを確認します。変更内容は状況によって異なります。

   変更を読みながら、TSDoc の doc comment、TypeScript の規約、コメントヘッダーに注目してください。これらは演習 2 の最初に確認したカスタム指示ファイルによるものです。

6. 更新内容をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローがキューに登録されたことを確認します。すべて問題なければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot との PR 作業は一方通行ではありません。チームのほかのメンバーと同様に、PR やコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを受け取ると、新しいセッションを開始して対応します。結果は非決定的なため、必ず同じ結果になる特定の指示はありません。たとえば、次の変更を依頼できます。

- 各コードファイルの先頭に、内容を簡潔に説明するコメントヘッダーを追加する。
- TypeScript と Astro ファイルに TSDoc の doc comment を追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを使い、生成されたドキュメントへの変更を依頼するコメントを追加します。ほかのユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択して Copilot の作業を確認します。Copilot は更新用に新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

   ![プルリクエストに戻るボタン](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

満足するまで改善を繰り返せます。問題がなければ、ドラフト PR をレビュー可能な状態にして `main` ブランチにマージします。

![PR をレビュー可能な状態にする操作](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue を解決するため、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちであることを示す表示を探します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

8. PR のチェック欄でワークフローがキューに登録されたことを確認します。すべて問題なければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。前と同様に、Copilot cloud agent に更新を依頼して反復できます。たとえば表示する関連ゲームの数を調整したり、コメントヘッダーや TSDoc の doc comment を追加したりできます (この Issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。**Conversation** タブにコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用のカスタムエージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で mission control から追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** が含まれます。完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - 表示モードを切り替える UI コンポーネント。
   - ユーザー設定を保存する local storage の使い方。
   - ハイコントラストとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容をレビューし、問題がなければ **Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す表示を探します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行するボタン](../../../_images/shared-approve-workflows.png)

9. PR のチェック欄でワークフローがキューに登録されたことを確認します。すべて問題なければ、プロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** Codespace で Copilot が作成したブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が保存されていることを確認してください。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに従うよう Copilot を導いたことを確認します。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** をメンションし、更新を依頼できます。

## 省略可能な演習 — さらに委任する

cloud agent は、バックログの実作業を任せて別の作業に取り組むときに効果を発揮します。リポジトリにいくつか Issue を登録し、Copilot に割り当ててみましょう。次のような課題があります。

- ゲーム詳細ページに支援者向けの関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハーネスを完了しました。これらのレッスンでは、次の内容を学習しました。

- **リポジトリに用意されたカスタム指示ファイルを確認し、cloud agent の出力への影響を確認できるようにした。**
- **Copilot cloud agent に Issue を割り当て、環境の準備、計画、非同期の実行を確認した。**
- **アクセシビリティ用のカスタムエージェントを作成して使い、ハイコントラストとライトモードの切り替えを追加した。**
- **Copilot agents ページを mission control として使い、作業中のアクセシビリティセッションを監視して方向付けた。**
- **cloud agent のプルリクエストをレビューして改善し、`@copilot` に変更を依頼し、ワークフローを承認した。**

## 振り返りと次のステップ

Cloud agent のハーネスは完了です。さらに学ぶ場合は、ほかのハーネスで異なる環境のシナリオを確認してください。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のことも試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の分野向けのカスタムエージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

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
