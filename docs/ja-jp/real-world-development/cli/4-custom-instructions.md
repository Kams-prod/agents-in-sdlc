---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント作成の標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合、そのガイダンスを Copilot が利用できるようにします。[指示ファイル][instruction-files]では、必要なコードだけでなく、コードの構成方法も伝えます。フィルタリング機能を作成したので、Copilot が使った指示を確認し、ドキュメント作成の標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス別指示ファイルがエージェントにどのように適用されるかを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントには、コードの言い換えではなく、意図や自明ではない判断を記述します。
- `db/` と `src/lib/` のエクスポート関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を追加します。該当する場合は、注入可能な `db` 引数も記述します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連するコードの変更時にはコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業できるように必要な情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや好みを Copilot に伝えられます。希望するコーディング規約、ライブラリ、コードコメントの形式なども指定できます。リポジトリ全体に適用する指示や、特定のファイル向けの指示を作成できます。

指示ファイルは 2 種類あります。

- `.github/copilot-instructions.md`: リポジトリへのすべてのリクエストで Copilot に送られる単一の指示ファイルです。多くのチャットや CLI リクエストに共通するプロジェクトレベルの情報を記載します。
- `.github/instructions/*.instructions.md`: 特定の言語、ファイルの種類、タスク向けのガイドラインを記載します。

> [!NOTE]
> ほかの指示形式への対応状況は環境によって異なります。特定の形式を利用する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、作業を始めやすいようにいくつかの指示ファイルが用意されています。変更を加える前に内容を確認し、変更の効果を比べてみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで（ターミナルではなく）`.github/copilot-instructions.md` を開きます。
3. プロジェクトの概要とコーディングガイダンスを確認します。この指示は、このリポジトリでの Copilot とのすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、ファイルを確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示を適用するファイルを決める glob を指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存のプロジェクトファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分割し、コード生成時に参考となる例を Copilot に示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、まだ不足があります。コアの `copilot-instructions.md` を変更し、新しく生成する TypeScript に TSDoc コメントを追加するよう指示します。

1. `.github/copilot-instructions.md` で **Code formatting guidance** セクションを見つけます。35 行目付近にあります。
2. 最後の箇条書きとして、次を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、会話のコンテキストを維持したままフィルタリングの会話を終了して再開し、Copilot に新しいガイダンスを読み込ませます。

1. フィルタリングの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に更新済みのガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新たに生成された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルのコンテキストをどのように読み込むかを確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパス別指示ファイルがエージェントにどのように適用されるかを確認しました。
- コーディング標準に従うよう指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、lint とテストが一貫して実行されるようにします。

## リソース

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルやその他のリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
