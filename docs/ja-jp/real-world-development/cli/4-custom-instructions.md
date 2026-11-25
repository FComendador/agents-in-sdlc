---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメントの標準を追加して、フィルタリングのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使う際、コンテキストは重要です。特定の方法でタスクを実行する必要があるなら、Copilot がその指示を参照できるようにしましょう。[指示ファイル][instruction-files]には、求めるコードの内容だけでなく、その構成方法も記述できます。フィルタリング機能を作成した今、Copilot が使用した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパス固有の指示がエージェントにどう届くかを確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発プラクティスに関するガイドラインと要件があります。

- コメントはコードの繰り返しではなく、意図や自明でない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数には、目的、パラメーター、戻り値を説明する TSDoc/JSDoc を記述します。注入可能な `db` 引数がある場合はそれも含めます。
- 再利用可能な Astro コンポーネントには `Props` コントラクトを記述します。関連するコードを変更した場合はコメントも更新します。
- 既存の書式設定と lint の指示は維持します。

指示ファイルを使い、Copilot がこれらのプラクティスに従うために必要な情報を提供します。

## 指示ファイル

カスタム指示は、スタイルや要件のコンテキストを Copilot に提供し、コードをより適切に理解させます。規約、ライブラリ、コメントの種類などを指定できます。リポジトリ全体、または特定のファイル形式やタスク向けの指示を作成できます。

指示ファイルには2つの種類があります。

- `.github/copilot-instructions.md` は、リポジトリに関するすべてのリクエストで Copilot に送信される単一ファイルです。大半のリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` には、特定の言語、ファイル形式、タスク向けのガイダンスを記載します。

> [!NOTE]
> 指示の形式とサポート状況は環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポート状況][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、すでにいくつかの指示ファイルがあります。変更を加える前に確認し、どのように機能するかを見てみましょう。

1. Codespace に戻ります。
2. Codespaces のエディターで、ターミナルではなく `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。これらは、このリポジトリでの Copilot とのすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いてファイルを確認します。Astro、Drizzle データレイヤー、テストなどに関する指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象を決める glob を定義しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルや既存のプロジェクトファイルへのリンクを確認します。これにより、大きな指示セットを小さく再利用可能なファイルに分け、Copilot が従うべき例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

既存のファイルはよい出発点ですが、ルールがひとつ不足しています。メインの `copilot-instructions.md` を変更し、新しい TypeScript コードに TSDoc コメントを含めるようにします。

1. `.github/copilot-instructions.md` で、35行目付近にある **Code formatting guidance** セクションを探します。
2. リストの最後に次の項目を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動保存されます。

## 更新した指示を適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存したら、フィルタリングの会話を終了して再開します。これにより、機能のコンテキストを保ったまま、新しいガイダンスを読み込めます。

1. フィルタリングの会話が開いている場合は、`/exit` と入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたら、フィルタリングの会話を選択し、`game-filters-cli` ブランチが選択されていることを確認します。
3. 更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` と入力して、変更された TypeScript ファイルを確認します。追加された TSDoc コメントがコードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからどのようにコンテキストを取得するかを確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの指示とパス固有の指示がエージェントにどう届くかを確認しました。
- コーディング標準に従うよう、指示ファイルを更新しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行し][next-lesson]、同じ lint とテストのチェックを行います。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート状況][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot: 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
