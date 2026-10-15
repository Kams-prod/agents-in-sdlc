---
slug: ja-jp/real-world-development/cloud
title: "GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

**[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent)** を使うと、GitHub Copilot がクラウドで非同期に作業できます。GitHub で作業を割り当てると、cloud agent はバックグラウンドでリポジトリを調査し、変更を加え、プルリクエストを作成します。その間、別の作業を進められます。

この一連の演習では、まず cloud agent が従うカスタム指示を追加し、GitHub Issue を割り当てて実装を任せます。エージェントの作業内容をレビューし、カスタムエージェントを使ってアプローチを調整し、agents ダッシュボードからセッションを監視して方向付けます。最後に、エージェントが作成したプルリクエストをレビューし、結果を改善します。

## 演習

| 演習 | トピック | 説明 |
|----------|-------|-------------|
| [0. 前提条件][ex0] | セットアップ | リポジトリと codespace を作成します |
| [1. カスタム指示][ex1] | コンテキスト | cloud agent が従うカスタム指示を追加します |
| [2. Cloud Agent][ex2] | 非同期エージェント | GitHub Issue を Copilot cloud agent に割り当てます |
| [3. カスタムエージェント][ex3] | 特化型エージェント | カスタムエージェントを確認して使用します |
| [4. エージェントの管理][ex4] | 監視 | エージェントセッションを監視して方向付けます |
| [5. 反復作業][ex5] | レビュー | プルリクエストをレビューし、Copilot の作業を改善して、次のステップを選びます |

## 前提条件

このワークショップに参加する前に、次の準備をしてください。

- [ ] 有効な **Copilot Student、Pro、Pro+、Business、または Enterprise** プランを利用できる GitHub アカウント
- [ ] アカウントまたは組織で有効になっている Copilot cloud agent

> [!TIP]
> 有料プランを利用していませんか？認証済みの学生は [GitHub Education][callout-student-plan-education] を通じて GitHub Copilot を無料で利用できます。**Copilot Student** プランには、このワークショップで使用するエージェント、MCP、コードレビュー、Copilot CLI の機能が含まれているため、どの演習も完了できます。

[callout-student-plan-education]: https://github.com/education/students

> [!NOTE]
> Cloud agent は Copilot Free を除く **Copilot Student、Pro、Pro+、Business、Enterprise** の各プランで利用できます。Copilot Business または Enterprise の場合は、管理者が組織で有効にする必要があります。

> [!NOTE]
> このハンズオンでは MCP を扱いません。Copilot で MCP サーバーを使う方法については、[CLI ハンズオン][cli-harness]または [VS Code ハンズオン][vscode-harness]を参照してください。

## はじめる

**[演習 0: 前提条件から開始 →][ex0]**

[ex0]: 0-prerequisites/
[ex1]: 1-custom-instructions/
[ex2]: 2-cloud-agent/
[ex3]: 3-custom-agents/
[ex4]: 4-managing-agents/
[ex5]: 5-iterating/
[cli-harness]: ../cli/
[vscode-harness]: ../vscode/
