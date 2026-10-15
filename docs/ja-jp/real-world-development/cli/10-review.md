---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、さらに学ぶためのリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローで GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを確認し、登録済みのフィルタリング Issue を見つけました。
- 星評価を追加して、転送されたブラウザーで結果を確認し、最初のプルリクエスト (PR) を手動でマージしました。
- フィルタリングの Issue から始め、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリング機能を試しました。
- 品質保証 (QA) カスタムエージェントを作成、選択し、要件、テストカバレッジ、スキルの結果、ブラウザーでの証拠を確認しました。
- フィルタリングの変更全体をレビューし、フィルタリングの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、cloud agent への任意の委任に使えるスラッシュコマンドを確認しました。

## リリースした内容

このワークショップには、それぞれ更新済みの `main` から作成した別々のブランチ上に、2 つの PR があります。

1. **星評価:** 既存の `starRating` と、評価がないことを明示する状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用します。`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

フィルタリングを計画してから PR を開くまで、同じ会話とブランチで作業しました。ワークショップを簡潔にするため、これらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot によるブラウザー操作など、いくつかの方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA エージェントは、プルリクエストの前に要件とテストカバレッジをレビューして、それらの結果をまとめました。

追加のテストは、実際に不足している部分を補うものであるべきです。QA を実行した結果、テストの追加が不要となる場合もあります。マージを許可する前にコードと根拠をレビューし、変更を加えた後は該当する検証結果を更新してください。

## ベストプラクティス

Copilot に提供するコンテキストとツールが、作業内容を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成して、MCP サーバーを設定しました。これらのカスタマイズを会話を越えて再利用し、チームのニーズに応じて調整してください。指示は標準を設定し、スキルは繰り返し可能なタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールと接続します。エージェントの概要だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。**Plan** は実装前の検討に、**Interactive** は対象を絞った変更を確認しながら進める場合に、**Autopilot** は範囲の明確なタスクに使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選んでください。

インフラストラクチャと同じくらい、コンテキストも重要です。*何を*作りたいのか、*なぜ*必要なのか、*どのように*進めるのかを明確に伝えることで、出力は大きく変わります。

## さらに試す

基本的なワークフローを学びました。Copilot CLI には、ほかにも便利な機能があります。

- `/review` はコードレビューエージェントに変更を分析させます。
- `/rubber-duck` は問題を説明して、別の視点からの意見を得るために使います。
- `/fleet` は独立したサブタスクを並行して調整します。
- `/worktree` は別のタスクを独立して作業するために使います。
- `/delegate` は Copilot cloud agent にタスクを送ります。

## 次のステップ

どのツールでも、スキルを高める最善の方法は使い続けることです。本番コード、趣味のコード、何年も頭の中にあったけれどまだ形にしていない小さなアプリなど、さまざまな場面で活用してください。学びをチームと共有し、チームからも学んでください。そして、いつものようにドキュメントを確認しましょう。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code のワークショップ][vscode]、[GitHub Copilot app のワークショップ][app]、[Copilot cloud agent のワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
