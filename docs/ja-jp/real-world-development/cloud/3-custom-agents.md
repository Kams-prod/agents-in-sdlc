---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや領域に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを置き、焦点を絞った指示、ベストプラクティス、コーディングパターン、領域固有の知識を定義すると、Copilot が特定の作業をより効果的に行えるようになります。チームの専門知識を再利用可能なエージェントとして共有できます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングプラクティスに従うセキュリティエージェント、一貫したテストパターンを維持するテストエージェントを作成できます。

カスタムエージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` に置いた Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義し、AI に作業方法を指示します。簡潔に区別すると、**カスタムエージェントは作業者で、スキルはツールです**。

カスタムエージェントは専用のコンテキストウィンドウを持ち、スキル（さらにはほかのエージェント）を調整して作業できます。このハンズオンでは、アクセシビリティカスタムエージェントがガイドラインに沿ってサイトをレビューし、更新します。その際、プルリクエストのワークフローを扱うスキルや、テストを実行、管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI のほかの機能と同じように、環境やシナリオに合わせてテストと改善を繰り返してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次のことを学びます。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て方

## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不足しているため、現在のダークテーマが読みづらいという指摘がありました。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントに機能を生成させます。
## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは作成済みです。内容を確認して、Copilot への指示を把握しましょう。

codespace に戻り、アクセシビリティカスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。ここでは、次の内容が説明されています。
   - アクセシブルな Web サイトのコードを生成する際の基本的な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある落とし穴や間違い
## Issue を作成して割り当てる

mission control は、環境内のすべてのエージェントを操作する中心的な場所です。Copilot cloud agent にタスクを割り当て、作業を監視し、追加の指示を与えられます。まず、高コントラストモードを作成するタスクを Copilot に割り当てます。

1. リポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択し、新しい Issue の作成ダイアログを開きます。
4. **Blank issue** を選択して、新しい Issue を作成します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。
9. 一覧から **Accessibility agent** を選択します。

    ![mission control で Accessibility が選択された、cloud agent の割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや領域に合わせた AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形で共有し、Copilot が特定の種類の作業をより効果的に行えるようにできます。

次の内容を学びました。

- カスタムエージェントの定義方法
- カスタムエージェントへのタスクの割り当て方

Copilot がハイコントラストモードの実装を進めている間に、[mission control からエージェントセッションを監視して方向付ける][next-lesson]方法を学びましょう。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [Enterprise でカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
