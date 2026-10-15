---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションへの最も簡単な更新でも、通常は複数のファイルを変更し、テストなどの操作を行う必要があります。開発者は、必要なファイルを探して変更し、テストを実行し、問題をデバッグし、変更を忘れたファイルを見つけて、さらに更新するという作業を繰り返します。作業は尽きません。

ここで Copilot のエージェントモードが役立ちます。

Copilot のエージェントモードは IDE 内でより自律的に動作します。開発者と同様に、既存のプロジェクト構造を調べ、必要な更新を実施し、テストなどのタスクを実行し、見つけた問題を自動的に修正します。エージェントモードを使ってサイトに新機能を追加する方法を見ていきましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途向けに作られています。エージェントモードは IDE 内で作業するため、すばやくフィードバックを得ながら対話できます。cloud agent はチームメンバーのように非同期で作業し、Issue とプルリクエストを通じてやり取りします。

この演習では、次の方法を学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連するファイルを見つけ、連携して変更を加える方法
- GitHub Copilot エージェントモードで UI とデータレイヤーにまたがる新機能を実装する方法
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法

## シナリオ

ゲーム一覧が増えてきたため、ユーザーがカテゴリやパブリッシャーで絞り込めるようにします。前の演習でパブリッシャーヘルパーを追加したので、Copilot エージェントモードで残りのデータレイヤー、UI、テストを仕上げます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の Web サイトを確認して、現在の機能を理解しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ユーザーはゲームの一覧と詳細を確認できます。単一の Astro アプリで、ビルド時にページを静的 HTML としてレンダリングします。Drizzle ORM を使い、ページの frontmatter からローカルの SQLite データベースに直接クエリを実行します。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` に置き、インタラクティブな UI は標準 DOM API を使う小規模な Astro `<script>` で追加します。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. codespace に戻ります。前の演習の後、`main` にいるはずです。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、Codespace 内に新しいターミナルを開きます。
3. フィルタリング作業用の新しいブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL を含むバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーウィンドウで開くよう求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択し、Web サイトを開きます。

> [!NOTE]
> codespace で作業している場合、Codespace ターミナルから localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは Web サーバーをホストする codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: ボードゲームの一覧と、各ゲームのタイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの詳細が表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なります。それぞれの手順で必要な場合に説明します。

Web サイトの初期実装は機能していますが、新しい機能を追加して改善したいと考えています。まずバックログを確認します。テンプレートからプロジェクトをセットアップしたときに Issue のバックログも自動的に作成されました。GitHub Copilot にこれらの項目を表示させましょう。

1. Chat ビューのエージェントのドロップダウンで **Agent** を選択します。**Agent** はファイルをまたいだ変更を自律的に計画、実装し、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。このワークショップの作成には Claude Sonnet 4.5 を使ったため、手順にも記載しています。受講者の体験が一定になるよう、比較的一貫したコード提案が得られることを期待しています。ただし、LLM は確率的に動作するため、ワークショップの記述と異なる提案が表示される場合があります。これは正常であり、想定された動作です。

> [!NOTE]
> LLM の確率的な性質により、Copilot は異なる MCP コマンドを使う場合がありますが、タスクは同様に完了できます。

3. 次のプロンプトを Copilot に送信して、Issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot がユーザーに役立つと思われる項目を基準に、優先順位も決めていることを確認します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、Copilot に作業のコンテキストを伝えるための指示ファイルを確認します。Web サイトの機能追加に取り組む方法が記載された[ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加への取り組み方に関する全体的なガイダンスを確認します。内容には次のものがあります。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト可能性、アクセシビリティの原則
   - 次のような、ファイルの種類ごとの指示へのリンク
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所を基準とします。指示を再利用したり、複雑な指示を扱いやすいサイズに分割したり、例やテンプレートを示したりするのに便利です。

## フィルタリング機能を実装する

フィルタリングを完成させるには、アプリケーションに少なくとも次の 3 つの変更が必要です。

- データレイヤー (`src/lib/`) に残りのフィルタリングロジックを追加、調整する
- フィルタリングの動作に関するテストを追加または更新する
- ゲーム一覧ページにフィルタリング UI を追加する

また、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェントモードを使うと、これらのタスクを実行できます。機能を追加しましょう。

1. Copilot の現在の会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択して、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例を示すスクリーンショット](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェントのドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

4. モデルに **Claude Sonnet 4.5** が引き続き選択されていることを確認します。
5. バックログの関連 Issue に基づいて機能を実装するよう、次のプロンプトを使って Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、目的の機能に関連するファイルを見つける様子を確認します。データレイヤーのヘルパーや UI、テストが対象になります。その後、Copilot はファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調査している様子を示すスクリーンショット](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトを調べ、ファイルを変更し、テストを実行するなど、複数のタスクを行います。タスクやコードベースの複雑さによっては数分かかる場合があります。その間に、コードエディターに **Keep** と **Undo** の操作が表示されることがあります。作業中は選択する必要はありません。完了すると、すべての変更に対する **Keep** または **Undo** が表示されます。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat ペインにテストを実行してよいか確認するダイアログが表示された状態](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に何度か一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストの間を行き来し、タスクが完了してエラーが見つからなくなるまで作業を繰り返します。

   ![Copilot エージェントモードの完全な Chat セッションを示すスクリーンショット](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに潜在的な問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を試します。
11. すべて正しく動くことを確認し、コードをレビューしたら、Copilot Chat ウィンドウの **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに機能を追加する方法を学びました。次のことを行いました。

- GitHub Copilot エージェントモードで UI とデータレイヤーにまたがる新機能を実装する方法
- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを見つけ、連携して変更を加える方法
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューする方法

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを開きます。

### 追加の演習 – ページネーションを実装する

ゲーム一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使い、Copilot にページネーションの実装を依頼してください。コードに関する検討事項は次のとおりです。

- 既存の指示ファイルを使うなど、現在のベストプラクティスに従います。
- ユーザーがページサイズを選べるようにするか、固定値を使うかなど、ページネーションの実装方法を検討します。
- 希望する実装に必要なガイダンスを Copilot に伝えるプロンプトを作成します。
- Copilot に変更を依頼したり、コンテキストを追加したりして、反復的な調整が必要になる場合があります。Copilot を使うときは通常このように進めます。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
