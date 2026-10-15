---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習で、ユーザーエクスペリエンスの改善と機能追加を目的とした 3 つのタスクを Copilot cloud agent に依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

GitHub.com の[エージェントページ][agents-page]など、cloud agent に割り当てたタスクを管理するツールがいくつかあります。この mission control では、オープンなプルリクエスト (PR) があるすべてのエージェントタスクを確認できます。実行内容を調べたり、進行中のセッションに指示を与えたりすることもできます。

このレッスンでは、次のことを行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼し、作業の方向を調整します。

## シナリオ

エージェントにハイコントラストモードの作成を依頼した後、チームはライトモードも追加するよい機会だと気付きました。サイトのスタイル更新と切り替え機能の追加をすでに進めているため、あわせて実装するのが合理的です。エージェントの作業の方向を調整して、ハイコントラストモードとライトモードの両方を追加してもらいます。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メインペインと左側のペインにタスクの一覧が表示されます。Copilot に割り当てたタスクとして、次のものがあることを確認します。
    - コードベースのドキュメントの更新
    - ゲームを変更する API の生成
    - Web サイトへのハイコントラストモードの追加
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。たとえば、次のような作業があります。
    - リポジトリのチェックアウト
    - Copilot が作業する環境の作成
    - MCP サーバーのセットアップ
    - タスクを完了するための各種手順の実行

> [!NOTE]
> 表示される正確な手順は、Copilot の作業状況や選択したアプローチによって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。ここで PR と変更されたファイルを確認し、さらに監視できます。

## cloud agent に指示を与える

実行中のタスクを確認できたので、ハイコントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. ハイコントラストモードの追加を示すセッションを選択します。タイトルは Copilot が使用した名前と作業状況によって異なります。

    ![mission control のアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業を開始したことが表示されるまで、数分間セッションを確認します。次のようなメッセージが表示されれば、作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![エージェントページで「Steer active session while copilot is working」ダイアログが強調表示された cloud agent タスクのスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業に取り込むことを確認します。

## Copilot の作業を待つ

これまでと同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進めていた手順が完了すると、新しい依頼が作業に反映されます。

この作業にも数分かかります。その間に、ここまでに学んだことを振り返ってみましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視し、方向付けるための中心的な場所である Copilot agents ページを使いました。この mission control で進行中や完了済みのタスクを追跡し、作業を確認し、進行中のタスクの範囲を変更したり、追加の指示を与えたりできます。

次の内容を学びました。

- mission control とエージェントページを使って cloud agent のタスクを監視する
- 進行中のセッションに追加機能を依頼する

Copilot がアクセシビリティ機能の作業を続けている間に、[Copilot が作成したプルリクエストを改善][next-lesson]しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
