---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合、その情報を利用できるようにします。特定のコンテキストを Copilot と共有する方法はいくつかあります。その中心となるのが[指示ファイル][instruction-files]です。指示ファイルを使うと、コード生成のガイダンスを伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示やパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認する。
- 現在の指示を適用した状態で、フィルタリング機能の最初のデータ部分 (publishers helper) を生成する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトを再実行し、生成コードに新しい標準が反映されることを確認する。
- 次の演習で新しい指示を使えるよう、指示の更新とフィルタリングの基礎を `main` にコミットする。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot は確率的に動作します。この演習の目的は、指示を更新した後の動作の**傾向**を確認することであり、出力を 1 文字ずつ一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。たとえば、次のとおりです。

- データレイヤーには必ずユニットテストを用意する。
- UI はダークモードにし、現代的な印象にする。
- コードのドキュメントには TSDoc の doc comment を使う。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加する。

指示ファイルを使い、Copilot に適切なコンテキストを与えて、これらのプラクティスに沿ってタスクを実行できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件を理解するためのコンテキストを Copilot に提供できます。好みのコーディング規約、ライブラリ、コードに含めるコメントなどを指定でき、提案の関連性を高められます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには 2 種類あります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、作成中のものの概要、ベストプラクティスなど、大半のチャットや CLI リクエストに関係するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの標準による指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法をすべて扱うことは、このワークショップの範囲外です。ただし、サンプルプロジェクトには代表的な方法が含まれています。

- `copilot-instructions.md` には、作成中のもの、プロジェクト構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関する指示に使います。
- 自然な言葉で明確に記述し、コードの望ましい例と避けるべき例を示します。

指示ファイルの作成方法や AI の使い方に唯一の正解はありません。実際に試し、プロジェクトに合った方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、指示ファイルを充実させることをお勧めします。このプロジェクトには、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けの指示ファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、環境によって方法が異なります。関連する手順は各環境のレッスンで説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

リポジトリに用意された指示ファイルを読みましょう。中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスにも注目してください。これらの指示は Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro、Drizzle データレイヤー、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。この glob はリポジトリルートからの相対パスで、指示が適用されるファイルを決めます。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで進みます。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内のファイルへのリンクがあります。大きな指示を再利用しやすい小さなファイルに分け、コード生成の参考例を Copilot に示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc の doc comment とファイルコメントヘッダーのルールを追加します。

## 変更用ブランチを作成する

ここでは、ベストプラクティスに従って作業用ブランチを作成します。

1. 前の演習で使った Codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、Codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めます。モードとモデルの選択は演習によって異なるため、該当する手順で説明します。

カスタム指示の影響を確認するため、まず現在の指示がある状態でプロンプトを送信します。その後、指示を更新して同じプロンプトを再度送信し、違いを確認します。

1. 前の演習のエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント選択欄で **Agent** を選び、ファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調査してコードを更新します。ヘルパーファイルとテストが変更されることもあります。
6. 提案されたヘルパーが、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc の doc comment とファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくても doc comment を追加する場合があります。その場合も問題ありません。この演習の目的は、指示を更新した後の動作の一貫性が高まることを確認することです。

## リポジトリ標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するファイルです。リポジトリのコーディング標準を記載し、コード提案の質を高めましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。ここにはコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパーには doc comment がありませんでした。
3. 既存の標準の直後に次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc doc comment を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルは全体的なガイダンス向けのリポジトリレベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。ここで追加した doc comment のルールのように、プロジェクト全体に適用する標準はリポジトリレベルのファイルに記載します。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選んで会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が対象ファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックがあることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 生成された関数に、次のような TSDoc の doc comment があることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用させました。このファイルは次の演習でも使用します。

## 指示と変更をコミットしてブランチを push する

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して新しいターミナルを開きます。
2. `git status` を実行し、指示の更新とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. ターミナルで、指示の更新と Copilot がヘルパーの基礎作成のために変更したファイルを stage してコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリに push します。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチを push したら、バックログにあるドキュメント標準の Issue と関連付けてプルリクエストを作成します。手動で行う代わりに、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼できます。Copilot に該当する Issue を見つけてもらい、Issue をクローズする PR を作成してマージします。

> [!NOTE]
> この演習では初めて Copilot に GitHub 上の操作を依頼します。プロジェクトテンプレートには GitHub ツールがすでに設定されています。Copilot が最初にツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習では、この接続に使われる Model Context Protocol について説明します。

1. Codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択欄で **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と、新しい publishers helper の基礎の両方を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して適切な場合に許可します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ブランチをローカルの `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作とシェルコマンドの **Allow** を求めたら、操作を確認して適切な場合に許可します。

GitHub Copilot の支援でプルリクエストを作成してマージできました。

## まとめと次のステップ

指示ファイルから Copilot がコンテキストを取得する仕組みを確認し、VS Code の Copilot Chat で次の作業を行いました。

- 既存の指示を使ってコード生成プロンプトを送り、生成結果を確認した。
- `.github/copilot-instructions.md` に新しいリポジトリ標準を追加した。
- 同じプロンプトを再実行し、提案されたコードに新しい標準が反映されることを確認した。
- 次の演習で使えるよう、指示の更新とヘルパーの基礎を `main` にコミットした。

次は、Copilot がコードベース全体に新機能を追加する[エージェントモード][next-lesson]で指示を使います。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
