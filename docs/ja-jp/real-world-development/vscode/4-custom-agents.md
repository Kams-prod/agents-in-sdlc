---
title: "演習 4 - カスタムエージェント"
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
## シナリオ

Tailspin Toys は、視覚的な能力や好みにかかわらず、誰もがクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近のユーザーフィードバックでは、テキストと背景色のコントラストが不足しているため、現在のダークテーマが読みづらいという指摘がありました。このアクセシビリティ上の課題に対応するため、デザインチームはユーザーが切り替えられるハイコントラストモードの実装を依頼しました。

アクセシビリティは重要なので、できるだけ早く実装したいと考えています。カスタムエージェントに機能を生成させます。

この演習では、次のことを行います。

- 既存のアクセシビリティカスタムエージェントを確認します。
- Copilot Chat でアクセシビリティエージェントを使って、ハイコントラストモードを実装します。

## アクセシビリティカスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントは作成済みです。内容を確認して、Copilot への指示を把握しましょう。

codespace に戻り、ターミナルを開いて、アクセシビリティ作業用に `main` から新しいブランチを作成します（演習 3 のフィルタリング PR とは分けて作業します）。

```bash
git checkout main
git pull
git checkout -b accessibility-vscode
```

1. `.github/agents/accessibility.md` を開きます。
2. `name` フィールドと `description` フィールドを含む YAML frontmatter を確認します。

> [!CAUTION]
> カスタムエージェントには `name` と `description` を含む frontmatter が必要です。

3. 続くセクションを確認します。ここでは、次の内容が説明されています。
   - アクセシブルな Web サイトのコードを生成する際の基本的な責任
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある落とし穴や間違い
## Copilot Chat でカスタムエージェントを使う

VS Code の Copilot Chat ビュー下部にあるエージェントのドロップダウンには、`.github/agents` で定義したすべてのカスタムエージェントが表示されます。カスタムエージェントを選択すると、そのエージェントの指示とツールに合わせてチャットセッションを設定できます。

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルは演習によって異なります。それぞれの手順で必要な場合に説明します。

1. エージェントのドロップダウンで **Agent** を選択します。すでに選択されている場合はそのまま続けます。

   ![Chat ビューのエージェント選択を示すスクリーンショット](../../../_images/shared-chat-mode-selector.png)

2. Chat ビュー下部にあるエージェントのドロップダウンを選択します。現在のエージェント（既定では **default**）が表示されています。
3. 利用可能なエージェントの一覧から **Accessibility agent** を選択します。
4. 次のプロンプトをアクセシビリティエージェントに送ります。

    ```plaintext
    Add a high-contrast mode to the site. There should be a toggle for high contrast which the user can set, and the setting should persist across page reloads using local storage on the browser.
    ```

5. Copilot Chat がコードベースを調べ、変更を提案して、プロジェクトファイルを編集します。各編集は、ファイルパスとレビューできる差分とともにチャット内に表示されます。
6. 編集が適用されると **Files changed** の表示が更新され、エージェントが変更したファイルを確認できます。

> [!NOTE]
> この作業には数分かかる場合があります。Copilot はリポジトリの実ファイルを変更します。Astro コンポーネントや CSS、関連するテストなどを編集します。

次の演習では、この作業の進行状況を確認し、必要に応じて方向を調整します。

## まとめと次のステップ

このレッスンでは、特定のタスクや領域に合わせた AI アシスタントである [GitHub Copilot のカスタムエージェント][custom-agents]について学びました。カスタムエージェントを使うと、チームの専門知識や標準を再利用可能な形で共有し、Copilot が特定の種類の作業をより効果的に行えるようにできます。

次の内容を学びました。

- カスタムエージェントの定義方法
- Copilot Chat のエージェントモードでカスタムエージェントを使う方法

次は、エージェントの作業を[監視して方向付けます][next-lesson]。変更が進む様子を確認し、同じセッションでライトモードの切り替えも追加します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [IDE でカスタムエージェントを作成する][creating-custom-agents-ide]
- [VS Code のカスタムエージェント][custom-agents-vscode]
- [カスタムエージェントの構成][custom-agents-config]
- [awesome-copilot のカスタムエージェント][awesome-copilot-agents]

---

| [← 前のレッスン: Playwright MCP サーバーで機能をテストする][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-mcp/
[next-lesson]: ../5-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[creating-custom-agents-ide]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/create-custom-agents-in-your-ide
[custom-agents-vscode]: https://code.visualstudio.com/docs/copilot/customization/custom-agents
[custom-agents-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
[awesome-copilot-agents]: https://github.com/github/awesome-copilot/tree/main/agents
