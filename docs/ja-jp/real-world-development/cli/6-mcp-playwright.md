---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーからフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くことだけが開発ではありません。データや外部サービスを扱い、Copilot が利用できる自動化機能を追加することもあります。ここで MCP サーバーが役立ちます。MCP サーバーを使うと、Copilot は CLI に組み込まれた機能を越えて、さまざまなツールやサービスを利用できます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びます。
- Playwright MCP サーバーが利用できない場合に追加します。
- エージェントにブラウザーを操作させて、フィルタリング機能を確認します。

## シナリオ

ユニットテストとエンドツーエンドテストは重要ですが、UI の更新を検証するには、実際に UI を操作する必要があります。Copilot がユーザーと同じように開発中の Web サイトを使えるようにして、変更が期待どおりに動作するかを確かめ、開発をさらに自動化します。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] は、AI エージェントが外部のツールやサービスと通信する方法を提供します。MCP を使うと、AI エージェントは外部ツールやサービスとリアルタイムで通信でき、最新情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

こうしたツールやリソースには、AI エージェントと外部ツールやサービスをつなぐ MCP サーバーを通じてアクセスします。各 MCP サーバーは、AI エージェントが利用できるツールやリソースのセットを提供します。

よく使われる MCP サーバーには、次のようなものがあります。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにも多くの MCP サーバーがあります。GitHub は、エコシステムの探索や貢献を促進するために [MCP レジストリ][mcp-registry]を運営しています。

> [!CAUTION]
> MCP サーバーはプロジェクトのほかの依存関係と同じように扱ってください。利用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は追加し、Copilot がユーザーと同じように Web サイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP をすでに構成済みの場合は、セットアップ手順をスキップして次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前に `playwright` と入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類が **STDIO** であることを確認し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押し、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright で機能を試すよう Copilot に依頼する

これまでは機能が期待どおりに動作することを手動で確認しました。今度は、Copilot が利用できる Playwright MCP サーバーを使って、同じことを行います。

1. Copilot に Playwright MCP サーバーを使って検証するよう、次のプロンプトを送ります。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> 通常は Copilot が MCP サーバーの利用方法を自ら判断するため、明示的に指示する必要はありません。ただし、使うべきツールが分かっているときは、伝えておくとよいでしょう。一貫した結果を得やすくなり、トークンの節約にもつながります。

2. ブラウザーを操作して機能を確認するために、Copilot が実行する手順を見ます。
3. レポートを読み、すべて期待どおりに動作していることを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作し、サーバーを停止して、結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を試せました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI がどのように使うかを学びました。
- Playwright MCP サーバーが利用できない場合に追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を確認しました。

次は、スキルとブラウザーツールを組み合わせた[QA カスタムエージェントを作成します][next-lesson]。

## リソース

- [MCP とは何か、なぜ注目されているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
