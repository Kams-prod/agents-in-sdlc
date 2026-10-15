---
title: "レッスン 7 - QA エージェントを作成して使う"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせた QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまで `quality-checks` スキルで自動チェックを実行し、Playwright MCP でブラウザー上のフィルタリング機能を確認しました。ここでは、これらの機能を明確な QA プロセスにまとめたカスタムエージェントを作成します。

このレッスンでは、次のことを行います。

- カスタムエージェントが指示、スキル、MCP ツールとどのように連携するかを確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、フィルタリングの Issue に基づく調査結果をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザー上の動作を一貫して確認したいと考えています。カスタムエージェントを使うと、QA のプロセスを調整し、再利用可能なレポートを作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の特化版です。プロファイルには、エージェントの目的、指示、利用可能なツールを記述します。このワークショップでは、`.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの指示ではチームの標準を説明します。`quality-checks` スキルには繰り返し実行できるチェックがまとめられています。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、Copilot がこれらの機能を使う方法を指示します。これらに代わるものではなく、別の会話も必要ありません。

## QA プロファイルを作成する

機能の PR を開く前に、再利用可能な QA プロファイルの作成を Copilot に依頼します。プロファイルには、QA が実行するチェックと、その作業範囲を定義します。

1. Codespace に戻り、フィルタリングの会話が Interactive モードであることを確認します。
2. 既定のエージェントに新しいカスタムエージェントを作成するよう依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使用する前に、そのプロファイルをレビューし、意図した QA ワークフローと制約を Copilot が設定したことを確認します。こうしておくと、検証だけを依頼したつもりが、エージェントに不完全な作業や範囲の広すぎる変更を行われるのを防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. frontmatter を読みます。`description` は必須です。`name` は省略できますが、記載するとエージェントを識別しやすくなります。
3. プロファイルの指示を読み、QA が要件から作業を始め、リポジトリの指示に従い、`quality-checks` スキルを実行して、Playwright MCP を使うことを確認します。
4. QA が根拠を添えて結果を報告し、実装コードを変更する前に許可を求め、変更をコミットせず、プルリクエストを作成しないことを確認します。
5. 責任や制約が抜けていたら、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話開始時にプロジェクトのエージェントを読み込みます。プロファイル作成後、同じフィルタリングの会話を再開して QA を選択します。これにより、コンテキストに含まれている Issue と計画時の決定を利用できます。

1. 次のプロンプトを使ってエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> エージェントを作成した直後は、利用可能なエージェントの一覧に表示されない場合があります。それでも、上記のコマンドで有効にできます。

2. 次のプロンプトを使い、QA エージェントに機能をレビューさせます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントの作業を待ちます。
4. 完了したら、レポートを読みます。

## まとめと次のステップ

再利用可能な専門家の役割をワークフローに追加し、その成果をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが指示、スキル、MCP ツールとどのように連携するかを確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選択し、フィルタリングの Issue に基づく調査結果をレビューしました。

実装、スキルの更新、QA プロファイル、テスト、検証レポートをレビューする準備ができました。次は、これらをまとめて[機能 PR を作成し、Agent Merge を使います][next-lesson]。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
