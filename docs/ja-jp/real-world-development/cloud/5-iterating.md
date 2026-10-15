---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このハンズオンでは、ユーザーエクスペリエンスの改善や機能追加を目的に、いくつかのタスクを GitHub Copilot と進めてきました。コードへのドキュメント追加、デザインチームが改善を検討できる関連ゲーム機能の作成、ハイコントラストモードとライトモードの切り替えを含むアクセシビリティ機能の実装を依頼しました。変更されたコードを確認し、必要に応じて Copilot にフィードバックを送り、チーム全体のレビューを有効にする前に作業を改善しましょう。

### シナリオ

これまで何度も説明したように、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスを実施する必要があります。ここでは、チームのレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot の提案を確認します。
## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は監督されずに非同期でタスクを実行するため、安全性を確保するためのセキュリティ制約が設けられています。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みはコード作成に使うブランチに限られます。
- cloud agent は GitHub Actions 内で動作し、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。MCP サーバーへのアクセスも含まれます。
## 生成されたドキュメントをレビューする

まず、GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR)、つまりコードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、ファイアウォールによって GitHub Copilot がブロックされたという警告が表示される場合があります。既定で外部リソース（外部 MCP サーバーへの呼び出しなど）へのアクセスが制限されているため、これは**想定どおり**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** またはそれに近いタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだタスクを実行中の場合、PR のタイトルに **[WIP]** と表示されます。その場合は Copilot の作業が完了するまで待ってください。数分かかる場合があります。休憩したり、ここまで学んだ内容を振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択して変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. TSDoc のドキュメントコメントなど、コードへの追加を確認します。具体的な変更内容は異なる場合があります。

    変更を確認するときは、演習 2 の最初に確認したカスタム指示ファイルに基づき、TSDoc のドキュメントコメント、TypeScript の規約、コメントヘッダーが追加されているかを確認します。

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. PR のチェックセクションにワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するとき、一方的に依頼するだけではありません。チームメンバーと同じように、プルリクエストやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確率的に変わるため、依頼する文章を厳密には指定できません。たとえば、次のような変更を依頼できます。

- それぞれのコードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例や、コードベースに追加したいドキュメントに関する提案を使って、**@copilot** にメンションし、生成されたドキュメントへの変更を依頼するコメントを追加します。
2. **View Session** を選択し、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

    ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して、変更をレビューします。

納得できるまで Copilot との反復作業を続けてください。満足できたら、PR を下書きからレビュー可能な状態に変更し、`main` ブランチにマージできます。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対応して、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** またはそれに近いタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローが承認待ちであることを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
8. PR のチェックセクションにワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストすることもできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名（例: **copilot/fix-8**）に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。ほかの作業と同じように、Copilot cloud agent に更新を依頼して反復的に改善できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます（このタスクは、カスタム指示を更新する**前**に割り当てたことを思い出してください）。前の PR と同様に、**Conversation** タブでコメントを追加すれば、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティカスタムエージェントを使って実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 で mission control から追加を依頼したライトモードの両方が含まれるはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** またはそれに近いタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR のタイトルに **[WIP]** と表示されます。その場合は Copilot の作業が完了するまで待ってください。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注意します。
   - モードを切り替える UI コンポーネント
   - ユーザー設定の保存に使用する local storage
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更
   - アクセシビリティ属性（ARIA ラベル、キーボード操作など）
   - モード切り替えを処理する JavaScript/TypeScript コード

6. 更新内容をレビューして問題がなければ、**Conversation** タブに戻って下にスクロールします。
7. ワークフローが承認待ちであることを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)
9. PR のチェックセクションにワークフローがキューに入ったことが表示されます。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。Codespace に移動し、ターミナルを開いて、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    その後、アプリケーションを起動して、ブラウザーでハイコントラストモードとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティカスタムエージェントが、アクセシビリティのベストプラクティスに沿って機能を実装できるよう Copilot を導いたことが分かります。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** にメンションして、更新を依頼できます。

## 任意の演習 — 委任を続ける

cloud agent は、バックログの実際の項目を任せて別の作業に集中する方法に適しています。習慣として身につけるために、リポジトリでいくつか Issue を作成して Copilot に割り当ててみましょう。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心登録フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent のハンズオンを完了しました。各レッスンを通じて、次のことを行いました。

- **このリポジトリのカスタム指示ファイルを確認し**、cloud agent の出力への影響を確認しました。
- **Issue を Copilot cloud agent に割り当て**、環境を準備して計画を立て、非同期でタスクを実行する様子を確認しました。
- **アクセシビリティ向けのカスタムエージェントを作成して使用し**、ハイコントラストモードとライトモードの切り替えを追加しました。
- **Copilot agents ページを mission control として使用し**、アクセシビリティセッションを監視して作業中に方向を調整しました。
- **cloud agent のプルリクエストをレビューして改善し**、`@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent のハンズオンを完了しました。ほかのハンズオンでは、ここで学んだ内容をさらに実践できます。

- 🖥️ **[VS Code ハンズオン](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP 統合を使います。
- 💻 **[CLI ハンズオン](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻るオプションの `/delegate` ブリッジを使い、ターミナルからレビュー済みの変更を 2 つリリースします。

自分のリポジトリでは、次の作業を試してください。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てる。
- 別の領域に対応するカスタムエージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
