---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織はほとんどありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot cloud agent は、コードの更新や機能の追加などを自律的に実行できます。作業が終わると、開発者がレビューできるようにドラフトの pull request を作成します。面倒な作業を任せて開発を加速し、より重要な課題に集中できます。

Copilot cloud agent を使って、次の内容を確認します。

- コード生成環境をカスタマイズする方法。
- 操作が安全に行われるようにする方法。
- スコープが明確な Issue の重要性。
- Issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には解決したい技術的負債があります。最初のサイトを作成した外部委託先は、ドキュメントを十分に作成していません。まず、アプリケーション内のエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加します。

デザイン チームは、ゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属するほかのゲームを関連ゲームとして表示し、閲覧を続けられるようにします。今は完成度の高い実装ではなく、UX の受け入れテストに使えるものを求めています。この作業は現在ブロッカーですが、より優先度の高い Issue もあります。

このような作業は優先度が下がりやすく、Copilot cloud agent に割り当てるのに適しています。開発者が別の作業に取り組む間、cloud agent は非同期に作業できます。後から戻って結果をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent の概要

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。人間の開発者に依頼するのと同じように、[GitHub Issue を Copilot に割り当て][assign-issue]ることもできます。割り当てると、Copilot は進捗を追跡するドラフト pull request を作成し、環境を準備して作業を開始します。作業中でも完了後でもセッションを確認できます。提案された解決策のレビュー準備ができると、Copilot は pull request であなたにメンションします。

## スコープが明確な指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単な操作でも、詳しく見ると複雑なことがあります。[Copilot cloud agent に依頼するタスクのスコープを慎重に決めてください][cloud-agent-best-practices]。AI ペア プログラマーとして扱い、段階を踏み、学び、試しながら調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot のガイド方法を確認しました。カスタム指示のハンズオンを完了していれば、`.github/copilot-instructions.md` の動作を確認したはずです。まだの場合は、cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認してください。後でその影響を確認できます。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - TypeScript の規約と TSDoc ドキュメント コメントの要件が記載された **Code standards** セクションを確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` frontmatter が `**/*.test.ts` ファイルに指示を適用することを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てたら、作成された pull request に TSDoc コメント、TypeScript の規約、ファイル ヘッダーが反映されているか確認します。後の演習で PR をレビューするときにも取り上げます。

## Copilot cloud agent の開発環境を設定する

誰が作業する場合でも、コードの作成には通常、専用の環境と初期状態を整える設定スクリプトが必要です。ソフトウェア エンジニアのように作業する Copilot にタスクを割り当てる場合も同じです。

cloud agent は作業環境として [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールと依存関係を準備できます。このワークショップでは、演習を進めやすくするため、あらかじめ設定してあります。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が利用できます。

```yaml
name: "Copilot Setup Steps"

# Allows you to test the setup steps from your repository's "Actions" tab
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # Set the permissions to the lowest permissions possible needed for *your steps*. Copilot will be given its own token for its operations.
    permissions:
      # If you want to clone the repository as part of your setup steps, for example to install dependencies, you'll need the `contents: read` permission. If you don't clone the repository in your setup steps, Copilot will do this for you automatically after the steps complete.
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # Frontend / app setup - Node.js (the whole app is now Astro + Drizzle/libSQL)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # Migrate + seed the local SQLite database so builds and tests have data.
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフロー ファイルと同じように見えますが、重要な点が 2 つあります。

- `copilot-setup-steps` という 1 つのジョブがあり、Copilot が pull request の作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うため、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するまで待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く認識されていますが、多くのプロジェクトには古い情報が残っていたり、ドキュメントがまったくなかったりします。このような技術的負債は放置されがちで、生産性を低下させ、コードベースの保守や新しい開発者の受け入れを難しくします。Copilot はドキュメントの作成に役立つため、この Issue を cloud agent に割り当てます。cloud agent がバックグラウンドでドキュメントを作成する間、後で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

    ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成された pull request (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような形式です。まだ表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が To Do リストを作成したことを確認します。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで数分かかる場合があります。Copilot は環境を準備し、前述のワークフローを実行し、プロジェクトを分析して、問題への最適な取り組み方を判断しています。

13. リストと、Copilot が実行するタスクを確認します。
14. pull request のタイムラインを下にスクロールし、Copilot が Issue の作業を開始したという更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示を更新するために、ウィンドウの再読み込みが必要な場合があります。

16. ライブ セッションをスクロールし、Copilot が問題を解決する様子を確認します。コードを調べて状態を把握し、計画を考えて決定し、コードを作成する過程を確認できます。

この作業には数分かかる場合があります。Copilot cloud agent の主な利点は、非同期に作業している間、開発者が別の作業に取り組めることです。この機能を活用して、もう 1 つタスクを割り当て、その間にアプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot cloud agent の利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームのセクションを追加する間にも、ほかの作業を進められます。時間があまりかからない作業でも、その時間を別の作業に使えます。このタスクも cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てのダイアログを開きます。

    ![Copilot を Issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業中であることを示す 👀 が Issue の最初のコメントに表示されます。

![目の絵文字で Issue の作業中であることを示す Copilot](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェア エンジニアのように作業するため、常に監視する必要はありません。完了後に結果をレビューします。次に、カスタム エージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents] を確認しました。Issue を Copilot に割り当てると、非同期に作業させることができます。技術的負債の解消、新機能の作成、フレームワーク間のコード移行などを依頼できます。

次の内容を確認しました。

- コード生成環境をカスタマイズする。
- 操作が安全に実行されるようにする。
- スコープを明確にした Issue の重要性。
- Copilot に Issue を割り当てる。

cloud agent がバックグラウンドで作業する間、カスタム エージェントを作成して使用できます。[Copilot cloud agent は MCP server も使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
