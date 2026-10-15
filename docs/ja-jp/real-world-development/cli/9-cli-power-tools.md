---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキスト管理、モデル選択、セッション共有、cloud agent への任意の委任に使えるスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

優れた CLI ツールと同じように、GitHub Copilot CLI にも操作用のスラッシュコマンドが数多く用意されています。高度な機能、内部情報、追加の構成オプションを利用できます。すでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。ここでは、ほかの便利なコマンドを紹介します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認します。
- `/model` を使って、利用できるモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認します。

## シナリオ

CLI での基本的なワークフローを完了しました。ここでは、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、ほかの機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルのコンテキストウィンドウの上限に達する場合があります。Copilot CLI は必要に応じて会話を自動的に圧縮しますが、スラッシュコマンドを使って自分で確認、圧縮することもできます。

1. Codespace に戻り、まだ開いていない場合はリポジトリのルートから Copilot CLI を起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどのように分かれているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合、目立った変化がないことがあります。

> [!NOTE]
> Copilot CLI はコンテキストウィンドウがいっぱいになると、自動的にコンテキストを圧縮します。圧縮するタイミングを選びたい場合は `/compact` を使います。無関係な別のタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選択する

モデルにはそれぞれ異なる強みがあり、開発者によって好みも異なります。Copilot CLI では、使いたいモデルを一覧から選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況を確認します。
3. 現在のモデルをそのまま使うか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力して学びを共有すると、AI ツールの活用方法を改善できます。`/share` コマンドを使うと、セッションを Markdown ファイルや HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist として公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにある `/share` のオプションを確認します。
2. セッションを共有する場合は、ローカルの Markdown にエクスポートする `/share file` など、用途に合った出力先を選択します。
3. エクスポートした内容を、誰かに送ったり公開したりする前に確認します。セッションのエクスポートには、プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有する意図のないリポジトリや会話の内容は公開しないでください。

## 任意: 作業を拡張または委任する

基本的なワークショップはこれで完了です。大きなタスクには、Copilot CLI の次のコマンドも使えます。

- `/fleet` は独立したサブタスクをサブエージェントに分け、並列に実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` はタスクを Copilot cloud agent に送り、非同期で作業させて、プルリクエストを作成できます。

これらのコマンドは worktree を追加したり、リモートで作業を開始したりするため、任意の内容として紹介しています。試す前に、範囲を明確にした新しいタスクを始めてください。結果は通常のワークフローに従ってレビューします。非同期エージェントによる作業を詳しく知りたい場合は、[cloud agent のワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、構成を変更し、セッションを共有し、内部の動作を確認できます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って、Copilot が会話のコンテキストを管理する方法を確認しました。
- `/model` を使って、利用できるモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使える任意のコマンドを確認しました。

Copilot CLI にはほかにも多くのスラッシュコマンドと機能があります。ここまでの内容を[振り返り][next-lesson]、次に学ぶことを確認して、ハンズオンを締めくくりましょう。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
