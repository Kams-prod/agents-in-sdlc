---
title: "レッスン 5 - quality-checks スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認して、レポート形式をカスタマイズし、フィルタリング機能の検証に使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書くことだけが開発ではありません。手動でコードの動作を確認し、指示ファイルを使って標準に従わせてきました。テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

こうした作業には、**エージェントスキル**が適しています。スキルを使うと、これらの操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次のことを行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys は、ユニットテスト、lint、型チェックに `quality-checks` スキルを使っています。チームでは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェントスキルには、再利用可能なタスクの指示、実行可能なスクリプト、エージェントが必要に応じて読み込む補助リソースをまとめます。基本的には、スキル名のフォルダーと、その中の Markdown ファイル `SKILL.md` で構成されます。Markdown ファイルには、そのスキルの名前と説明を定義する frontmatter、概要、スキルを呼び出すタイミングに関するガイダンスが含まれます。スキルから呼び出すスクリプトや、そのほかのリソース用にサブフォルダーを追加することもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトにある既存のコマンドを使うため、`SKILL.md` だけで構成されています。

スキルをプロジェクトの `.github/skills` フォルダーに配置すると、リポジトリのアセットとしてチームで共有、再利用できます。また、ユーザーのスキルフォルダー `~/.copilot/skills` に配置することもできます。

## スキルを確認する

Tailspin Toys のチームがユニットテスト、lint、型チェックの実行用に作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. 先頭にある `name` と `description` を読みます。説明により、Copilot がスキルを呼び出すタイミングを判断します。
3. 指示を読み、テストと lint の作業手順がどのように記載されているかを確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接実行することも、自然言語で依頼することもできます。Copilot に 3 つのチェックを実行させましょう。

1. フィルタリングの会話に Interactive モードで戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

どの作業を実行したか、成功したか、ツールからどのような結果が報告されたかが分かるレポートに改善しましょう。スキルを更新して、結果を報告させます。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. ファイル末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

更新したスキルを試してみましょう。Copilot CLI は会話を再起動せずに、編集済みのスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次のことを行いました。

- ユニットテスト、lint、型チェックを行う `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能 PR のフィルタリング機能と一緒に含まれます。次は [Playwright MCP サーバー][next-lesson]を使って、Copilot にサイトを直接操作させます。

## スキルのその他の例

コミュニティによる次の例は参考資料であり、追加の作業ではありません。

- [Agent Skills の仕様][skill-spec]
- [コントリビューションワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポートスクリプトを含む `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
