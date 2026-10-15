---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題や、更新が必要なレガシーコード、実装する時間がなくバックログに残り続ける機能リクエストなどがあります。GitHub Copilot の cloud agent は、コードの更新や機能の追加といったタスクを自律的に実行できます。エージェントが作業を完了すると、人間の開発者がレビューできる下書き PR が作成されます。面倒な作業を委任して開発を加速し、より大きな課題に集中できます。

この演習では、Copilot cloud agent を使って次のことを行います。

- コード生成の環境をカスタマイズします。
- 操作を安全に実行する方法を確認します。
- Issue の範囲を明確にすることの重要性を学びます。
- Issue を Copilot に割り当てます。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。サイトの初期バージョンを作成した開発業者は、ドキュメントを十分に整備していませんでした。実際のところ、ドキュメントはまったくありません。まずは、アプリケーション内のすべてのエクスポート関数に TSDoc のドキュメントコメントを追加します。

また、ゲームのデザインを改善する準備も整いました。各ゲームの詳細ページに、同じカテゴリに属するほかのタイトルを「関連ゲーム」として表示し、閲覧を続けられるようにしたいと考えています。まだ洗練された実装は必要なく、UX の受け入れテストに使える程度で十分です。これは現在ブロッカーになっていますが、より優先度の高い Issue がほかにあります。

どちらも、優先順位を下げられがちな作業の例です。Copilot cloud agent に割り当てれば、開発者が別のタスクに集中している間に非同期で作業させ、後から結果をレビューして期待どおりか確認できます。
## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じように、バックグラウンドでタスクを実行できます。人間の開発者に作業を依頼するのと同様に、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)など、複数の方法があります。割り当てると、Copilot は進捗を追跡する下書きプルリクエストを作成し、環境をセットアップして、タスクに取り組みます。作業中でも完了後でも、Copilot のセッションを確認できます。解決案をレビューする準備が整うと、プルリクエストでユーザーにメンションします。
## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省けるわけではありません。一見簡単そうな操作も、詳細を見ていくと複雑な場合があります。[Copilot cloud agent に依頼するタスクの範囲を明確にすること][cloud-agent-best-practices]が重要です。AI ペアプログラマーとして扱い、段階的に作業し、学びながら試し、必要に応じて調整してください。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot の動作を導く方法を紹介しました。カスタム指示の演習をすでに完了している場合は、`.github/copilot-instructions.md` の動作を見ています。まだの場合は、この機会に確認してください。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

リポジトリの github.com の Web UI、または起動済みの codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md`: **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md`: `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成されたプルリクエストに TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが反映されているか確認してください。後の演習でプルリクエストをレビューするときにも説明します。

## Copilot cloud agent の開発環境をセットアップする

コードを作成するときは、誰が作業する場合でも、通常は適切な状態で作業を開始するための環境やセットアップスクリプトが必要です。同じようにソフトウェアエンジニアとして作業する Copilot にタスクを割り当てる場合も、環境の準備が必要です。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。作業を始める前に実行する[特別なセットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成すると、環境をカスタマイズできます。これにより、必要な開発ツールや依存関係を利用できるようになります。ハンズオンを進めやすくし、この仕組みを学べるように、あらかじめ設定済みです。Copilot が Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、単一の Astro アプリ用に移行およびシードされた SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *手順*に必要な最小限のアクセス許可を設定します。Copilot は自分の操作用に独自のトークンを取得します。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js（アプリ全体は Astro + Drizzle/libSQL です）
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに必要なデータを用意するため、ローカルの SQLite データベースを移行してシードします。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じですが、いくつかのポイントがあります。

- `copilot-setup-steps` という 1 つのジョブが含まれています。このジョブは、Copilot がプルリクエストで作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot の実行を待たず、ワークフローが正常に動くことをテストするのに便利です。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、多くのプロジェクトでは情報が古いか、ドキュメントがまったくありません。このような技術的負債が放置されると生産性が下がり、コードベースの保守や新しい開発者の参加が難しくなります。Copilot はドキュメント作成を得意としているため、cloud agent に割り当てるのに適した Issue です。必要なドキュメントをバックグラウンドで生成します。次の演習でその成果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue の作成ダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような名前になります。新しい PR が一覧に表示されない場合は、少し待ってからブラウザーウィンドウを更新します。
12. 数分後、Copilot が TODO リストを作成したことを確認します。

> [!NOTE]
> Copilot が PR に TODO リストを表示するまで、数分かかる場合があります。Copilot は先ほど説明したワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組むための適切な方法を検討しています。

13. リストと、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue に取り組み始めたことを示す更新を確認します。
15. **View session** ボタンを選択します。

  ![Copilot セッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されるまで、ウィンドウの更新が必要な場合があります。

16. ライブセッションをスクロールして、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を検討し、コードを作成する流れが見られます。

この作業には数分かかる場合があります。Copilot cloud agent の主な目的の 1 つは、タスクを非同期に実行して、別の作業に集中できるようにすることです。この機能を活用するため、cloud agent に別のタスクも割り当ててから、アプリケーションに機能を追加するコードを書くことに取り組みます。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。デザインチーム向けの関連ゲームの追加に時間がかからない場合でも、その時間を別の作業に使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue の作成ダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

  ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

すぐに、Issue の最初のコメントに 👀 が表示され、Copilot が作業中であることが示されます。

![Copilot が Issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェアエンジニアと同じように動作するため、作業中は常に監視する必要はありません。完了したらレビューしてください。次は、カスタムエージェントの作成と使用について学びます。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てると、非同期でタスクを実行できます。技術的負債への対処、新機能の作成、あるフレームワークから別のフレームワークへのコード移行などに Copilot を使えます。

次の内容を学びました。

- コード生成の環境をカスタマイズする
- 操作を安全に実行する方法を確認する
- Issue の範囲を明確にすることの重要性を学ぶ
- Issue を Copilot に割り当てる

cloud agent がバックグラウンドで作業している間に、カスタムエージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも使用できます][cloud-agent-mcp]。また、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
