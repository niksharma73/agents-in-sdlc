---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]には、どのようなコードを作るかだけでなく、どのように構成するかも記述できます。フィルタリング機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用しましょう。

このレッスンでは、次の内容を学習します。

- リポジトリの指示やパス単位の指示ファイルがエージェントに適用される仕組みを確認する。
- コーディング標準に従うため、指示ファイルを更新する。
- 指示ファイルがコードに与える影響を確認する。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のとおりです。

- コメントには、コードの言い換えではなく、意図や自明でない判断を記述する。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値、および該当する場合は注入可能な `db` 引数を説明する TSDoc/JSDoc を付ける。
- 再利用可能な Astro コンポーネントには `Props` の契約を記述し、関連するコードの変更時にはコメントも最新に保つ。
- 既存のフォーマットと lint のガイダンスを維持する。

指示ファイルを使い、Copilot に必要なコンテキストを与えて、これらのプラクティスに沿ってタスクを行えるようにします。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件を理解するためのコンテキストを Copilot に提供できます。好みのコーディング規約、ライブラリ、コードに含めるコメントなどを指定でき、提案の関連性を高められます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。大半のリクエストに関係するプロジェクトレベルのコンテキストを記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスク向けのガイドラインを記載します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトの指示ファイルを確認する

スタータープロジェクトには、すでに指示ファイルが用意されています。変更を加える前に、既存のファイルを確認しましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡潔な説明とコーディングガイダンスを確認します。これらの指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob によって、指示が適用されるファイルが決まります。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルやプロジェクト内のファイルへのリンクを確認します。大きな指示を再利用可能な小さなファイルに分け、コード生成の参考例を Copilot に示せます。

## チームのガイダンスに合わせて指示を更新する

既存のファイルを更新して、生成される TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** という見出しを探します。35 行目付近にあります。
2. 次の項目を末尾に追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。フィルタリング機能のコンテキストを保ったまま、新しい指示を読み込ませるため、会話を終了して再開します。

1. フィルタリングの会話が開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   求められた場合はフィルタリングの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードの内容を正確に説明していることを確認します。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行します。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
