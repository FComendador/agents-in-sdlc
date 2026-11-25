---
title: "演習 1 - カスタム指示 (cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。作業の方法が決まっている場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストにアクセスできるようにします。[指示ファイル][instruction-files]を使うと、Copilot は実行する内容だけでなく、その方法も理解できます。

この演習では、次の操作を行います。

- リポジトリのカスタム指示とパスに応じた指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI の環境とは異なり、ここでは変更前後を比較するプロンプトを実行しません。GitHub Copilot cloud agent は GitHub Issue に対して非同期で動作するため、リアルタイムで並べて効果を示すのが難しいためです。この環境の後半で cloud agent が作成した pull request をレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

ほかの開発チームと同じように、Tailspin Toys にも開発プラクティスに関するガイドラインと要件があります。

- データ レイヤーには必ず単体テストを用意します。
- UI はダーク モードにし、現代的な見た目にします。
- コードには TSDoc のドキュメント コメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使い、これらのプラクティスに沿って作業するために必要な情報を Copilot に伝えます。

### カスタム指示

カスタム指示は、コーディング スタイルや要件を Copilot が理解しやすくなるよう、コンテキストや設定を伝えます。コーディング規約、ライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体、または特定のファイルの種類やタスクに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発中の内容の概要、ベスト プラクティス、全般的なガイダンスなど、多くのチャットや CLI のリクエストに関係するプロジェクト レベルの情報を記載します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類に合わせて作成します。TypeScript や Astro などの言語別、UI コンポーネントの作成や単体テストの追加などのタスク別にガイドラインを設定できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われ、コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、コード生成時にリポジトリ レベルの指示と、`applyTo` frontmatter を持つ `*.instructions.md` ファイルの両方を使います。
>
> Copilot は AGENTS.md や CLAUDE.md など、ほかの標準の指示ファイルにも[対応しています][custom-instructions-support]。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成方法を詳しく説明することは、このワークショップの範囲外です。サンプル プロジェクトには、代表的な方法を示す例があります。基本的には、次の点を意識します。

- `copilot-instructions.md` には、開発内容の説明、プロジェクト構成、全体的なコーディング標準など、プロジェクト レベルのガイダンスを記載します。
- `*.instructions.md` ファイルには、単体テスト、Astro コンポーネント、データ レイヤーなどのファイルの種類別、またはタスク別の指示を記載します。
- 自然な言葉で明確に説明し、コードの望ましい例と避けるべき例を示します。

指示ファイルの作成方法に唯一の正解がないように、AI の使い方にも唯一の正解はありません。試しながら、プロジェクトに合った方法を見つけましょう。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイル一式を用意することをお勧めします。このプロジェクトのファイルには、[UI の更新][ui-instructions]や [Astro][astro-instructions] など、多くのタスク向けの指示があります。
>
> Copilot に指示ファイルを作成してもらうこともできます。方法は環境によって異なります。たとえば、VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init` を使います。使用中の環境に応じたレッスンで、必要な場合に手順を説明します。
>
> テンプレートや出発点を探す場合は、指示ファイル、カスタム エージェントなどのリソースをまとめた [awesome-copilot][awesome-copilot] リポジトリを確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読みます。基本となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開いてください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** の指示にも注目します。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ レイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、リポジトリのルートからの相対 glob を指定し、指示の適用対象を決めます。ここでは、`**/*.test.ts` に一致する TypeScript のテスト ファイルが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクを確認します。大きな指示を小さく再利用可能なファイルに分割し、コード生成時に Copilot が参考にする例を示せます。ここに記載されたパスはリポジトリのルートではなく、指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc コメントとファイル ヘッダーのルールを追加します。

## リポジトリ全体に適用する標準を追加する

この演習で行う唯一の編集として、TSDoc コメントとファイル レベルのコメント ヘッダーをコードに含めるプロジェクト全体のルールを追加します。この環境の後半で cloud agent に Issue を割り当てると、このルールが適用されます。

編集する前に、作業用のブランチを作成します。cloud agent は Issue の対象ブランチから指示を読み取りますが、変更を整理してコミットします。

1. codespace のターミナルで新しいブランチを作成し、切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト レベルの情報を伝えます。コードの提案を改善するため、リポジトリのコーディング標準を記載します。

1. `.github/copilot-instructions.md` をもう一度開きます。
2. 27 行目付近にある **Code formatting requirements** セクションを探します。コーディング標準が記載されていますが、コード内のドキュメントに関するルールがまだないことを確認します。
3. 既存の標準の直後に次の Markdown を追加し、ファイル ヘッダーと TSDoc コメントを Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、全般的なガイダンスにはリポジトリ レベルのファイル (`.github/copilot-instructions.md`)、特定の言語、ファイルの種類、タスクには `*.instructions.md` ファイルを使います。今回追加したドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示の変更をコミット、プッシュ、マージする

cloud agent は Issue の対象ブランチから指示ファイルを読み取ります。次の演習で Issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。cloud agent が新しい指示を使えるように、変更を `main` に反映します。

1. 変更をステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` への pull request を作成し、マージします。次の演習で Issue を割り当てると、cloud agent はこの指示を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して、`main` に直接コミットしてもかまいません。実際のプロジェクトでの作業方法を反映するため、ここではブランチを作成します。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。この環境の後半で、cloud agent が作成する pull request にこの標準が適用される様子を確認します。

次は、[最初の Issue を GitHub Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を書くための 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
