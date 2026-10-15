---
title: "レッスン 8 - 機能 PR を作成してマージする"
description: "フィルタリング機能、指示、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストが 1 つのブランチに保存されています。まとめてレビューして、プルリクエストを作成しましょう。星評価のプルリクエスト (PR) は自分でマージしましたが、今回は **Agent Merge** に作業を任せます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは流れを簡略化するため、フィルタリングと品質に関する作業を 1 つの会話とブランチにまとめ、この PR に含めました。

このレッスンでは、次のことを行います。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びます。
- 機能全体の変更と検証の根拠を確認します。
- フィルタリング機能の PR を作成します。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされたことを確認します。

## シナリオ

フィルタリング機能の開発を通じて、Copilot で機能を計画、実装、検証しました。Tailspin Toys では、マージの承認を開発者が管理したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge とは

**Agent Merge** は、プルリクエストをマージするために必要な残りの作業を自動化します。有効にすると、Copilot は CI チェックの失敗の修正、レビューコメントへの対応、必要に応じたリベースなど、PR をブロックしている問題を解決します。その後、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその役割を任せることもできますが、有効化する前に作業をレビューし、マージ可能な状態であると判断してください。

## Agent Merge で PR を管理する

コードの作成が終わりました。まとめてレビューして PR を作成し、Agent Merge に残りの作業を任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。ブロックされた場合は、報告された理由を確認し、リポジトリで許可される方法でレビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの作成など、開発プロセスの複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージのライフサイクルを自動化する仕組みを学びました。
- 機能全体の変更と検証の根拠を確認しました。
- フィルタリング機能の PR を作成しました。
- レビュー後にのみ Agent Merge を有効にして、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、cloud agent へのオプションの委任に使える [Copilot CLI の便利なスラッシュコマンド][next-lesson]を学びます。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
