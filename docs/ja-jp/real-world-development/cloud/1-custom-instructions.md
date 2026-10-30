---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で進める必要がある場合や、Copilot に知っておいてほしい背景情報がある場合、その情報を利用できるようにします。[指示ファイル][instruction-files]を使うと、何をしてほしいかだけでなく、どのように進めてほしいかも伝えられます。

この演習では、次の内容を学習します。

- リポジトリのカスタム指示やパス単位の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる仕組みを確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここではプロンプトの変更前後を比較しません。Copilot cloud agent は GitHub Issue に対して非同期に作業するため、リアルタイムで並べて効果を示すのは困難です。このハーネスの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

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

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、プロジェクトの概要、ベストプラクティスなど、大半のチャットや CLI リクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別向けに作成します。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示ファイルと、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は、AGENTS.md や CLAUDE.md など、[ほかの標準による指示ファイル][custom-instructions-support]もサポートしています。

### 指示ファイルを管理するためのベストプラクティス

このワークショップでは指示ファイルの作成方法をすべて扱いませんが、サンプルプロジェクトには代表的な方法が含まれています。

- `copilot-instructions.md` には、作成中のもの、プロジェクト構造、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` は、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種別や、特定のタスクに関する指示に使います。
- 自然な言葉で明確に記述し、コードの望ましい例と避けるべき例を示します。

指示ファイルの作り方や AI の使い方に唯一の正解はありません。試しながらプロジェクトに合った方法を見つけてください。

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

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中核となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルが含まれています。エディターまたは GitHub の Web UI で確認しましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡潔な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。これらの指示は Copilot とのすべてのやり取りに適用されます。
3. `.github/instructions` フォルダーを開きます。Astro、Drizzle データレイヤー、テストなどの指示が含まれています。
4. `.github/instructions/unit-tests.instructions.md` を開き、先頭の `applyTo` フィールドを確認します。これはリポジトリルートを基準とする glob で、指示が適用されるファイルを決めます。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開いて末尾まで進みます。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内のファイルへのリンクがあります。大きな指示を再利用しやすい小さなファイルに分け、コード生成時に参考にする例を示せます。ここに記載されたパスはリポジトリルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で TSDoc の doc comment とファイルコメントヘッダーのルールを追加します。

## リポジトリ標準を追加する

この演習で編集するのは、コード内のドキュメントとファイルレベルのコメントヘッダーをプロジェクト全体のルールとして追加する作業です。後の演習で Issue を割り当てると、cloud agent はこの指示に従います。

編集の前にブランチを作成します。cloud agent は Issue の対象ブランチにある指示を読みますが、変更を整理してコミットするため、ここではブランチを使います。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コーディング標準を記述して、提案の質を高めましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。既存の標準は記載されていますが、コード内のドキュメントに関するルールがありません。
3. 既存の標準の直後に、次の Markdown 行を追加し、ファイルコメントヘッダーと TSDoc の doc comment を Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで見たように、指示ファイルは全体的なガイダンス向けのリポジトリレベル (`.github/copilot-instructions.md`) にも、特定の言語、ファイル種別、タスク向けの `*.instructions.md` にも作成できます。ここで追加した doc comment のルールのように、プロジェクト全体に適用する標準はリポジトリレベルのファイルに記載します。

## 指示の変更をコミット、push、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に Issue を割り当てると、`main` からブランチが作成されます。cloud agent が更新した指示を使えるように、変更を `main` に取り込みます。

1. 変更を stage してコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチを push します。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチの作成を省略して、`main` に直接コミットします。ブランチの手順は、実際のプロジェクトでの作業方法を反映しています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを取得する方法を確認し、リポジトリ全体の標準を `.github/copilot-instructions.md` に追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューすると、この標準が適用されていることを確認できます。

次は、[Copilot cloud agent に最初の Issue を割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示をより良く書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
