---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前の演習: 前提条件][previous-lesson] · [次の演習: エージェントモード →][next-lesson]

生成 AI を使う際、コンテキストは重要です。タスクを特定の方法で進める必要がある場合や、Copilot に背景情報を知らせたい場合は、そのコンテキストを参照できるようにします。コンテキストを共有する主な方法の一つが[指示ファイル][instruction-files]です。指示ファイルを使ってコード生成を導きます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパス指定の指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準が Copilot に届く仕組みを確認します。
- 現在の指示を使って、フィルタリング用の最初のデータ部分となる publisher ヘルパーを生成します。
- `.github/copilot-instructions.md` にリポジトリ全体の新しい標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準を採用する様子を確認します。
- 次の演習で Copilot が更新済みの指示を使えるよう、指示の更新とフィルタリングの基盤を `main` にコミットします。

> [!CAUTION]
> 生成コードが一部の標準と異なる場合があります。Copilot は確率的に動作します。この演習の目的は指示を更新した後の動作の傾向を確認することであり、出力を一字一句一致させることではありません。

## 指示ファイル

### シナリオ

Tailspin Toys には開発プラクティスに関するガイドラインと要件があります。

- データレイヤーには必ずユニットテストを用意します。
- ユーザーインターフェイス (UI) はダークモードで、現代的な外観にします。
- コードに TSDoc ドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの内容を説明するコメントブロックを追加します。

指示ファイルを使い、Copilot がこれらのプラクティスに沿って作業するための情報を提供します。

### カスタム指示

カスタム指示は、スタイルや要件のコンテキストを Copilot に提供します。規約、ライブラリ、使用するコメントの種類などを指定できます。リポジトリ全体、または特定のファイル形式やタスク向けに指示を作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一ファイルです。技術スタック、プロジェクトの概要、ベストプラクティス、全般的なガイダンスなど、Copilot Chat や CLI の大半のリクエストに関係する情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式向けのガイドです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに使えます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent はコード生成時にリポジトリレベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルを使用します。
>
> Copilot は AGENTS.md や CLAUDE.md など、他の標準の指示ファイルにも[対応しています][custom-instructions-support]。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成方法の詳細はワークショップの範囲外ですが、サンプルプロジェクトには代表的な方法が示されています。

- `copilot-instructions.md` は、構築中のもの、プロジェクト構造、全体的なコーディング標準など、プロジェクトレベルのガイダンスに絞ります。
- `*.instructions.md` はユニットテスト、Astro コンポーネント、データレイヤーなどのファイル形式や、特定タスク向けに使います。
- 自然言語で明確に記述し、コードの望ましい例と避けるべき例を示します。

指示ファイルの作成にも AI の使用にも唯一の正解はありません。試しながら、プロジェクトに合う方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使用するすべてのプロジェクトには、十分な指示ファイルを用意するとよいでしょう。このプロジェクトには [UI の更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを生成してもらうこともできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、環境ごとに方法が異なります。該当する演習で説明します。
>
> テンプレートを探す場合は、指示ファイル、カスタムエージェントなどを集めた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリには、中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で確認しましょう。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの概要や **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** の指示にも注目します。これらは Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開き、Astro、Drizzle データレイヤー、テストなどの指示ファイルを確認します。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` は、リポジトリのルートからの glob を定義します。たとえば `**/*.test.ts` に一致する TypeScript のテストファイルに適用されます。
5. プロジェクト固有のユニットテスト作成ガイドを確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾にスクロールします。`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクを確認します。大きな指示セットを小さく再利用可能なファイルに分け、Copilot が従うべき例を指定できます。パスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクト標準が記載されていますが、コード内のドキュメント化はまだ求めていません。次の手順で TSDoc コメントとファイルヘッダーのルールを追加します。

## 変更用ブランチを作成する

変更を分離するため、新しいブランチを作成します。

1. 前の演習の codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) でターミナルを開きます。
3. ブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する*前に* Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以降の手順を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選び、新しい会話を始めます。モードやモデルの選択は演習によって異なり、必要な箇所で案内します。

まず現在の指示を適用した状態でプロンプトを送ります。その後ファイルを更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを使うようにします。
2. ヘルパーを配置する場所を示すため、`src/lib/publishers.ts` を開きます。
3. Chat ビューのエージェントドロップダウンで **Agent** を選択し、ファイル変更を適用できるようにします。

   ![Chat ビューのエージェント選択](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを確認してコードを更新します。ヘルパーファイルとテストが変更されることがあります。
6. 提案されたヘルパーが型付き関数で、最初の引数に `db` クライアントを受け取り、型付きの publisher 配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 提案されたコードに TSDoc コメントとファイルヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する場合があります。その場合でも、指示を更新した後の一貫性の向上がこの演習の要点です。

## リポジトリ全体の標準を追加する

`.github/copilot-instructions.md` はプロジェクトレベルの情報を Copilot に提供します。コード提案を改善するため、ドキュメント標準を追加します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。現在はコード内のドキュメント化のルールがありません。
3. 既存の標準の直下に、ファイルヘッダーと TSDoc コメントを求める次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> リポジトリレベルの `.github/copilot-instructions.md` は全体的なガイダンスに、`*.instructions.md` は特定の言語、ファイル形式、タスクに使います。今回追加したドキュメントコメント規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行し、変化を確認する

更新済みの指示を適用した状態で同じプロンプトを送り、提案を比較します。

1. Copilot Chat に戻り、**New Chat** を選択して会話コンテキストをクリアします。
2. `src/lib/publishers.ts` を再度選択し、Copilot が対象ファイルに注目するようにします。
3. 前回と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが含まれることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しい標準に従うよう Copilot を導き、次の演習につながる実際のコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のリポジトリアセットと同様にソース管理します。変更をコミットしてブランチをプッシュします。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) で新しいターミナルを開きます。
2. 次を実行し、指示とヘルパーの変更を確認します。

   ```bash
   git status
   ```

3. 指示の更新と、ヘルパーの基盤として Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## pull request を作成してマージする

ブランチをプッシュしたので、ドキュメント標準の issue に関連付けた pull request (PR) を作成します。Copilot に GitHub issue を探させ、issue をクローズする PR を作成してマージすることもできます。

> [!NOTE]
> この演習では初めて Copilot が GitHub 上の操作を行います。プロジェクトテンプレートでは GitHub ツールがすでに接続されているため、設定は不要です。初回の利用時に VS Code から GitHub へのサインインを求められることがあります。案内に従ってください。後の演習では、この接続に使われる Model Context Protocol (MCP) を確認します。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択で **Agent** が選ばれていることを確認します。
3. 指示ファイルの更新に関する issue を探し、現在のブランチから PR を作成して、指示の更新とフィルタリング作業向けの publisher ヘルパー基盤の両方を記載するよう Copilot に依頼します。

   ```plaintext
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成する様子を確認します。
5. GitHub の操作について **Allow** を求められたら、コマンドを確認して許可します。
6. PR が作成されたら、`main` にマージし、ローカルでも `main` に戻って最新コードを取得するよう Copilot に依頼します。

   ```plaintext
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub の操作やシェルコマンドの実行に **Allow** を求められたら、内容を確認して許可します。

GitHub Copilot の支援で pull request を作成し、マージできました。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを得る方法を確認し、VS Code の Copilot Chat で次のことを行いました。

- 現在の指示でコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。
- 同じプロンプトを再送信し、提案コードに新しい標準が適用される様子を確認しました。
- 次の演習で使えるよう、指示の更新とヘルパーの基盤を `main` にコミットしました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体の機能を追加させます。

## リソース

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム指示を書く5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot: 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
