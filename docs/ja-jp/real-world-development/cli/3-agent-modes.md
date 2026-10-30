---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で実装方針を合意し、Autopilot で Issue に基づいてフィルタリングを構築して、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は小さな機能をプロジェクトに追加しました。より大きな変更には、さらに確かなプロセスが必要です。GitHub Copilot CLI は組織の既存のワークフローに沿って使えるため、適切なものを適切な方法で構築できます。ここから数レッスンでは、Issue を起点に新機能を作成し、コードの妥当性と機能の動作を確認して、最終的にプロジェクトにマージする、一般的なエージェント主導の開発プロセスに取り組みます。

> [!NOTE]
> 機能開発の流れを続ける間、同じ会話とブランチを使います。通常はファイルの種類ごとに異なるブランチや PR を使いますが、ここでは主な概念に集中するため簡略化します。

このレッスンでは、次の内容を学習します。

- GitHub Issue から Copilot の会話を始める。
- Plan モードで要件を決める。
- Autopilot モードで新機能を実装する。
- コードをレビューする。
- 転送されたブラウザーで機能を手動で検証する。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP 検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増えてきたため、訪問者がカテゴリーやパブリッシャーでゲームを絞り込めるようにします。バックログの Issue には機能が記載されていますが、カテゴリーの組み合わせ方など、コーディング前に合意が必要な詳細があります。Plan モードで決定してから、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発に取り入れても、基本的な手順は変わりません。むしろ、いっそう重要になります。多くの開発者は、次のような流れで作業します。

1. 必要な作業を説明する Issue から始めます。
2. 実装する内容を計画します。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功したら、変更をマージします。

> [!NOTE]
> チームや組織によって詳細は異なりますが、多くの場合は上記の流れに近いものです。

この標準的な進め方に従えば、AI が生成したコードも要件を満たし、手作業で作成したコードと同じレビューと検証を受けられます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押してモードを切り替えます。

- **Interactive**: エージェントと一緒に作業します。エージェントは変更を提案し、次に進む前に入力を待ちます。
- **Plan**: エージェントが先に計画を立てます。プロジェクトファイルの編集はできません。
- **Autopilot**: エージェントがコードの作成、テストの実行、反復を自律的に行い、タスクの完了を目指します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## Issue から始める

フィルタリングの作業を始める前に、Codespace に戻り、リポジトリとターミナルを準備します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 星評価の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルタリング用ブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての Issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** という Issue を選びます。
9. <kbd>C</kbd> を押して Issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の Issue やプルリクエストを会話のコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てると、機能の実装やタスクの実行を Copilot に任せる前に、方針を決められます。複雑な作業では、事前に計画を立てることをお勧めします。Plan モードに切り替え、Copilot に計画を作成してもらいましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 前の手順で追加した Issue の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調査して、実装方法を検討し、計画を作成します。

3. フィルタリングの動作について質問されたら、希望する動作を回答します。正解は 1 つではありません。
4. 計画ができたら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を読み、内容を確認します。
6. 決定した内容と合わない部分があれば、Copilot に計画の修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたら、実装に進みます。Autopilot を使って Copilot に作業を任せます。

Autopilot では、Copilot がタスクを完了したと判断するまで、問題に取り組みながら反復します。

1. **Accept plan and build on autopilot (recommended)** またはインストール済みのバージョンで同様のラベルの項目を選びます。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まる場合があります。先に計画を確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前にセットアップの問題を解決します。

## 実装をレビューして検証する

コードの生成後は、ほかのコードと同様にマージ前にレビューします。コードを確認し、サイトを実行して動作も確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードにします。モード表示に **Plan** または **Autopilot** がないことを確認します。
2. `/diff` を入力して、フィルタリングの実装とテストを調べます。
3. 実装を Issue および計画で決めた内容と照らし合わせます。
4. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新しい機能を試す

コードに問題がないように見えても、実際に動くでしょうか。前と同じようにアプリを起動し、Codespaces で転送されたポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画で合意したカテゴリーとパブリッシャーのフィルタリング、組み合わせを試します。
4. リセットと該当する結果がない場合の動作が、Issue および決定事項に合っていることを確認します。
5. Codespace に戻り、Copilot が起動した開発サーバーを停止するよう依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を構築し、レビューしました。このレッスンでは、次の内容を学習しました。

- GitHub Issue から Copilot の会話を始めた。
- Plan モードで要件を決めた。
- Autopilot モードで新機能を実装した。
- コードをレビューした。
- 転送されたブラウザーで機能を手動で検証した。

次は、[カスタム指示を使って][next-lesson]、コードが文書化されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
