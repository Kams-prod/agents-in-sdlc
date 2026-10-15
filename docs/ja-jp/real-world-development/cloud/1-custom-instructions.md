---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、必要なコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に作業内容だけでなく、作業方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。

> [!NOTE]
> VS Code と CLI のハンズオンとは異なり、ここでは変更前と変更後を比べるプロンプトを実行しません。Copilot cloud agent は GitHub Issue に対して非同期で作業するため、その影響をリアルタイムで比較しにくいためです。このハンズオンの後半で、cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- データレイヤーには必ずユニットテストを作成します。
- UI はダークモードにして、現代的な見た目にします。
- TSDoc のドキュメントコメントを使ってコードにドキュメントを追加します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot が適切な情報を得て、これらのプラクティスに沿って作業できるようにします。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや好みを Copilot に伝えられます。希望するコーディング規約、ライブラリ、コードコメントの形式などを指定し、より適切な提案やコードスニペットを得ることができます。リポジトリ全体に適用する指示や、特定のファイル種類やタスクに適用する指示を作成できます。

指示ファイルは 2 種類あります。

- `.github/copilot-instructions.md`: リポジトリへの**すべて**のリクエストで Copilot に送信する単一の指示ファイルです。技術スタック、構築するものの概要、ベストプラクティス、その他のグローバルなガイダンスなど、多くのチャットや CLI リクエストに共通するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル種類向けの指示ファイルです。TypeScript や Astro などの言語や、UI コンポーネントやユニットテストの作成などのタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの指示と、コード生成時に `applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、ほかの標準に基づく指示ファイルにも対応しています。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

### 指示ファイルを管理するためのベストプラクティス

このワークショップでは指示ファイルの作成方法をすべて説明できませんが、サンプルプロジェクトの例から代表的なアプローチを確認できます。基本として、次の点に留意してください。

- `copilot-instructions.md` には、構築するものやプロジェクトの構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル種類や、特定のタスクに関する指示を記載します。
- 自然な言葉で明確に指示します。コードの望ましい例と望ましくない例を示します。

指示ファイルの作成方法にも、AI の使い方と同じく唯一の正解はありません。実際に試しながら、プロジェクトに合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトの指示ファイルには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、さまざまなタスク向けのものがあります。
>
> Copilot に指示ファイルの生成を依頼することもできます。各環境での方法は異なります（たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` です）。このハンズオンでは、該当するレッスンで説明します。
>
> テンプレートや作成を始めるための資料を探している場合は、指示ファイル、カスタムエージェント、その他のリソースが集められたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに用意されている指示ファイルを読み、内容を把握しましょう。`.github/copilot-instructions.md` が 1 つと、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要や、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認します。これらの指示は、Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決める glob を指定します。ここでは、`**/*.test.ts` に一致する TypeScript テストファイルが対象になります。
5. このプロジェクトのユニットテスト作成に関する指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内のファイルへのリンクがあります。これを使って、大きな指示を再利用可能な小さなファイルに分け、コード生成時に参考例を Copilot に示せます（ここにあるパスはリポジトリルートではなく、指示ファイルを基準としています）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順では、TSDoc のドキュメントコメントとファイルのコメントヘッダーを追加するルールを設定します。

## リポジトリの新しい標準を追加する

ここで行う編集は、TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーを使って、コード内にドキュメントを記載するという、プロジェクト全体のルールの追加です。後の演習で Issue を割り当てるとき、cloud agent がこのルールに従います。

編集する前に、作業用ブランチを作成します（cloud agent は Issue の対象ブランチにある指示を読み込みますが、変更を整理してコミットするためです）。

1. Codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。コード提案を改善するため、リポジトリのコーディング標準にドキュメント作成ルールを追加しましょう。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。既存のコーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパー関数にもドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加して、ファイルのコメントヘッダーと TSDoc のドキュメントコメントを指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル（`.github/copilot-instructions.md`）に配置して全体的なガイダンスを示すことも、特定の言語、ファイル種類、タスク向けに `*.instructions.md` として作成することもできます。ここで追加したドキュメントコメントのルールのような、プロジェクト全体の標準にはリポジトリレベルのファイルが適しています。

## 指示の更新をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。そのため、cloud agent が指示を利用するには、変更を `main` にマージする必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開いてマージします。次の演習で Issue に割り当てたとき、cloud agent がこの指示を読み取れるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業することもできます。その場合はブランチの作成を省略し、`main` に直接コミットしてください。ここでブランチを使うのは、実際のプロジェクトでの作業方法を再現するためです。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。このハンズオンの後半で cloud agent が作成するプルリクエストをレビューするときに、この標準がどのように使われるかを確認できます。

次は、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズのための指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot に適切なカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
