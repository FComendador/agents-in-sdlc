---
title: "レッスン 5 - 品質チェックのスキルをカスタマイズして使う"
description: "`quality-checks` スキルを確認し、結果の形式をカスタマイズして、フィルター機能の検証に使用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コードを書く作業には、単にコードを書く以上のことが含まれます。コードが動作することを手動で検証し、指示ファイルを使って規約に従うようにしてきました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このようなタスクには、**エージェント スキル**が最適です。スキルを使うと、こうした処理を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の操作を行います。

- 既存の `quality-checks` スキルを確認します。
- 結果の形式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、単体テスト、lint、型チェックに `quality-checks` スキルを使用しています。チームは、結果を読みやすくするためにレポートを改善したいと考えています。

## 指示、スクリプト、リソース

エージェント スキルには、再利用可能なタスクの指示、実行可能なスクリプト、エージェントが必要に応じて読み込む補助リソースがまとめられています。基本的には、スキル名のフォルダーと、その中にある `SKILL.md` という Markdown ファイルで構成されます。このファイルには、スキルの名前と説明を含む frontmatter、機能の概要、呼び出すタイミングのガイダンスが記載されています。フォルダーには、スキルが呼び出されたときに使用するスクリプトなどのリソースを含むサブフォルダーも追加できます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の `quality-checks` スキルは、プロジェクトに既存のコマンドを使用するため、`SKILL.md` だけで構成されています。

スキルは、チームで共有して再利用できるようにプロジェクトの `.github/skills` フォルダーに配置するか、ユーザーのスキル フォルダーである `~/.copilot/skills` に配置できます。

## スキルを確認する

Tailspin Toys のチームが単体テスト、lint、型チェックを実行するために作成した `quality-checks` スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで `.github/skills/quality-checks/SKILL.md` を開きます。
2. ファイルの先頭にある `name` と `description` を読みます。説明は、スキルを呼び出すタイミングを Copilot が理解するのに役立ちます。
3. 指示を読み、テストと lint の手順をどのように Copilot に示しているか確認します。
4. スキルに **Results output formatting** セクションがまだないことを確認します。

## 変更前にスキルを実行する

スキルは Copilot CLI から直接呼び出すことも、自然言語で指示することもできます。Copilot にスキルの 3 つのチェックを実行するよう依頼します。

1. **Interactive** モードで、フィルター機能についての会話に戻ります。
2. 次のプロンプトを使用します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に表示されるレポートを確認します。

## レポートをカスタマイズする

実行した内容、成功したかどうか、ツールが実際に報告した内容を示すレポートに改善しましょう。そのレポートを生成するようにスキルを更新します。

1. `.github/skills/quality-checks/SKILL.md` に戻ります。
2. 次のセクションをファイルの末尾に追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を確認しましょう。Copilot CLI は、会話を再起動せずに編集済みのスキルを再読み込みできます。

1. 次を入力します。

   ```plaintext
   /skills reload
   ```

2. 前とまったく同じプロンプトを使用します。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェント スキルをカスタマイズして使用しました。このレッスンでは、次の操作を行いました。

- 単体テスト、lint、型チェック用の `quality-checks` スキルを確認しました。
- 結果の形式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、機能の pull request でフィルター機能とともに反映されます。次は、[次のレッスンで紹介する Model Context Protocol (MCP) 対応の Playwright server][next-lesson] を使って Copilot がサイトを直接操作できるようにします。

## スキルのその他の例

コミュニティの次の例は参考資料であり、追加のタスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図とエクスポート スクリプト: `drawio`][drawio-example]
- [ブラウザー テスト: `webapp-testing`][browser-example]

[previous-lesson]: ../4-custom-instructions/
[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
