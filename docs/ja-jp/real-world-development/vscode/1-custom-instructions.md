---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行する必要がある場合や、Copilot が知っておくべき背景情報がある場合は、必要なコンテキストを参照できるようにします。Copilot と特定のコンテキストを共有する方法はいくつかあります。その中でも重要なのが[指示ファイル][instruction-files]です。コード生成のガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス別指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示を使って、フィルタリングに必要な最初のデータ処理（パブリッシャーヘルパー）を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する新しい標準を追加します。
- 同じプロンプトをもう一度送信し、新しい標準に従うコードが生成されることを確認します。
- 次の演習で Copilot が更新されたガイダンスを使えるように、指示の更新とフィルタリングの処理を `main` にコミットします。

> [!CAUTION]
> 生成されたコードが、設定した標準の一部に従わない場合があります。Copilot の動作は確率的です。この演習では、出力を一字一句同じにするのではなく、指示の更新後に動作がどのように変わるかを確認します。

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
- `.github/instructions/*.instructions.md`: 特定のタスクやファイル種類向けの指示ファイルです。TypeScript や Astro などの言語や、UI コンポーネントや新しいユニットテストの作成などのタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE で作業する場合、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
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
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などのほかの指示ファイルや、プロジェクト内のファイルへのリンクがあります。これを使って、大きな指示を再利用可能な小さなファイルに分け、コード生成時に参考となる例を Copilot に示せます（ここにあるパスはリポジトリルートではなく、指示ファイルを基準としています）。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順では、TSDoc のドキュメントコメントとファイルのコメントヘッダーを追加するルールを設定します。

## 変更用のブランチを作成する

ここではベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する**前**に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なります。それぞれの手順で必要な場合に説明します。

カスタム指示の影響を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを見ます。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開きます。ヘルパーを配置する場所を Copilot に伝えます。
3. Chat ビューのエージェントのドロップダウンで **Agent** を選択し、Copilot がファイルに変更を適用できるようにします。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。多くの場合、ヘルパーファイルとそのテストの両方が変更されます。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数であることを確認します。これは、`src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に従っています。
7. 提案されたコードには TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**含まれていない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。指示を更新した後の一貫性の向上が、この演習の目的です。

## リポジトリの新しい標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクトレベルの情報を提供します。より適切なコード提案を得るため、リポジトリのコーディング標準を文書化しましょう。

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

## プロンプトをもう一度送信して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して入力内容をクリアします。
2. `src/lib/publishers.ts` を選択して、Copilot が適切なファイルに集中できるようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次の例のようなコメントブロックが追加されたことを確認します。

   ```typescript
   /**
    * Tailspin Toys のクラウドファンディングプラットフォーム用パブリッシャーデータアクセスヘルパーです。
    * データベースからパブリッシャー情報を取得する関数を提供します。
    */
   ```

5. 提案された関数に、次の例のような TSDoc コメントが追加されたことを確認します。

   ```typescript
   /**
    * ID と名前を含むすべてのパブリッシャーの一覧を返します。
    *
    * @param db - Drizzle データベースクライアントです。
    * @returns パブリッシャーオブジェクトの配列に解決される promise です。
    */
   ```

Copilot に新しいプロジェクト標準を伝え、次の演習でも利用する実際のコードに適用できました。

## 指示をコミットしてブランチをプッシュする

指示ファイルもリポジトリのアセットです。ほかのファイルと同じソース管理の方法で扱います。更新内容をコミットしてブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルウィンドウを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. ターミナルで次のコマンドを実行し、指示の更新と、ヘルパーの基盤作成のために Copilot が変更したすべてのファイルをステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status に追加の補助的な変更（テストなど）が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. 次のコマンドでブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、ドキュメント標準に関する Issue と関連付けたプルリクエストを作成します。手動で作成することもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に任せましょう。Issue を見つけ、Issue をクローズする PR を作成してから、PR をマージするよう Copilot に依頼します。

> [!NOTE]
> この演習では、初めて Copilot に GitHub 上の操作を任せます。プロジェクトのテンプレートには GitHub のツールがワークスペースに接続されているため、セットアップは不要です。Copilot が初めて GitHub ツールを使うとき、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従って許可してください。後の演習では、この接続で使われる Model Context Protocol について学びます。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェントのドロップダウンで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関連する Issue を探し、指示の更新と新しいパブリッシャーヘルパーの基盤作成の両方を説明する PR を現在のブランチから作成するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を探して PR を作成します。
5. Copilot に代わって GitHub の操作を実行する **Allow** プロンプトが表示されたら、コマンドを確認して適切に許可します。
6. PR が作成されたら、次のプロンプトを使って PR をマージし、ブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot に代わって GitHub の操作とシェルコマンドを実行する **Allow** プロンプトが表示されたら、コマンドを確認して適切に許可します。

GitHub Copilot の支援を受けて、プルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成用のプロンプトを送り、Copilot の提案を確認する。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加する。
- 同じプロンプトをもう一度送り、新しい標準に従うコードが提案されることを確認する。
- 次の演習で利用できるよう、指示の更新とヘルパーの基盤を `main` にコミットする。

次は、Copilot がコードベース全体に新機能を追加する[エージェントモード][next-lesson]で、指示を活用します。

## リソース

- [GitHub Copilot のカスタマイズのための指示ファイル][instruction-files]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot に適切なカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
