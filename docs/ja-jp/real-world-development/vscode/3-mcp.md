---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使って、フィルタリング機能を作成しました。プルリクエストを開く前に、ブラウザーで機能が正しく動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して Copilot に実際のブラウザーを操作させ、機能をテストします。その後、ブランチを公開して PR を開きます。

この演習では、次のことを行います。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP を使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開して、フィルタリングのプルリクエストを開きます。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部と連携できるとさらに強力になります。Copilot がこれを実現する方法が Model Context Protocol (MCP) です。エージェントが外部のツールやサービスと通信するための標準です。

![エージェントモードの内部構造と、MCP サーバーおよび VS Code 拡張機能が提供するツールを含む、コンテキスト、LLM、ツールとの連携を示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP を使うことで、AI エージェントはリアルタイムに外部のツールやサービスと通信し、リソースを使って最新情報にアクセスしたり、ツールを使ってユーザーに代わり操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーは、AI エージェントと外部ツール（既存の API や npm パッケージなどのローカルツール）との通信を管理します。それぞれの MCP サーバーは、AI エージェントが利用できる異なるツールとリソースを提供します。

よく使われる MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を利用できます。AI エージェントは、新しいリポジトリの作成、既存のリポジトリの更新、Issue やプルリクエストの管理などを行えます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。AI エージェントは、Web ページへの移動、フォームへの入力、ボタンの選択などを実行できます。

ほかにもさまざまなツールやリソースを利用できる MCP サーバーがあります。GitHub は、エコシステムの探索や貢献を促進するために [MCP レジストリ](https://github.com/mcp)を運営しています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトのほかの依存関係と同じように扱ってください。利用前にソースコードを十分に確認し、発行元を検証して、セキュリティへの影響を検討してください。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可する場合は慎重に判断してください。
## MCP の構成を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されていることを確認します。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクトテンプレートに含まれているため、前の演習で Copilot はバックログの Issue を読み込めました。[リモート GitHub MCP サーバー][remote-github-mcp-server]を使うので、ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot がサーバーのツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと Copilot がブラウザーを操作できるため、機能のテストに適しています。

1. `.vscode/mcp.json` の `github` と同じ階層に `playwright` エントリを追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると、画面を表示せずに Playwright がブラウザーを実行します。デスクトップ画面のない codespace では、この指定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンドテストにも Playwright を使っているため、通常はブラウザーがすでにインストールされています。Copilot からブラウザーがないと報告された場合は、`npx playwright install chromium` を実行してから再試行してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がサーバーのツールを初めて必要としたときに、VS Code はサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** 操作が表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、構成を確認し、サーバーの起動を許可します。

手動での起動は任意です。スキップした場合は、次の手順で Copilot がツールを初めて使うときに VS Code がサーバーを起動し、信頼するか確認します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot は実際のブラウザーを操作できます。自分でアプリを操作して作業を確認する代わりに、エージェントにページを開かせ、移動やフィルターの適用を行わせ、その結果を報告させることができます。会話から離れずに機能の動作を確認できる、最も簡単な方法です。

Playwright MCP サーバーは、スクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使って動作します。つまり、エージェントは支援技術と同じように、ラベル付きの構造化された要素（ボタン、リンク、リスト項目など）に基づいて処理します。そのため、簡単な機能チェックをアクセシビリティの基本確認としても利用できます。

サーバーを接続し、アプリを起動した状態で、Copilot にフィルタリング機能を確認させます。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、手順に沿って操作し、結果を報告します。レポートを Issue の受け入れ条件と照らし合わせます。問題がある場合は、プルリクエストを開く前に追加の質問をするか、コードの修正を依頼します。

> [!NOTE]
> テスト時には、アプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止している場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP サーバーを初めて使用するときにブラウザーをダウンロードする場合があります。ブラウザーがないと報告された場合は、`npx playwright install chromium` を実行してから再試行するよう依頼してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
## ブランチを公開してプルリクエストを作成する

機能が正しく動くことを確認できたので、チームがレビューできるようプルリクエスト (PR) を開きます。まず `filtering-vscode` ブランチを公開します。

1. codespace で **Source Control** パネルに移動し、Copilot が作成した変更をレビューします。
2. **+** アイコンを選択して変更をステージングします。
3. **Sparkle** ボタンを使ってコミットメッセージを作成します。

   ![Copilot が作成した変更を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択して、ブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) を使うなど、プルリクエストを作成する方法はいくつかあります。ここでは GitHub Copilot を使って PR を作成します。Copilot は関連する Issue を見つけ、Issue と関連付けた PR を作成できます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェントのドロップダウンで **Agent** が選択されていることを確認します。これにより Copilot が GitHub ツールを使えます。
3. Copilot に PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報を収集して操作を実行できるよう Copilot に許可します。GitHub ツールを初めて使うときは、GitHub での認証が求められる場合があります。画面の指示に従って許可します。
5. Copilot が Issue を検索して適切なものを見つけ、PR を作成します。
6. Copilot が生成したリンクを選択してプルリクエストをレビューしますが、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) とは何か、MCP サーバーが新しいツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- Copilot に Playwright MCP を使わせ、リリース前にブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開して、フィルタリングのプルリクエストを開きました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティレビューなどに特化した作業を効率化するために、[カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
