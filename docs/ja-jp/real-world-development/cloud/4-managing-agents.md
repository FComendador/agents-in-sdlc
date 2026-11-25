---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

前の 2 つの演習では、ユーザー エクスペリエンスを改善して機能を追加するため、GitHub Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、タスクを監視することも重要です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェント ページ][agents-page]など、さまざまなツールで管理できます。このミッション コントロールでは、未完了の pull request があるエージェントのタスクをすべて確認できます。実行された操作を調べたり、進行中のセッションに追加のガイダンスを送ったりできます。

このレッスンでは、次の操作を行います。

- エージェント ページを使って cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードを依頼した後、チームはライト モードも追加するよい機会だと考えました。サイトのスタイルとモード切り替え機能をすでに更新しているため、ライト モードも追加するのが自然です。エージェントに追加の指示を送り、両方のモードを実装するようにします。

## GitHub Copilot cloud agent のタスクを確認する

cloud agent に割り当てたすべてのタスクの現在の状態を確認しましょう。

1. [Copilot エージェント ページ](https://github.com/copilot/agents)を開きます。
2. メイン パネルと左側のパネルにあるタスク一覧を確認します。Copilot に割り当てた次のようなタスクが表示されます。
   - コードベースのドキュメントを更新する。
   - ゲームを変更する API を生成する。
   - Web サイトにハイ コントラスト モードを追加する。
3. 実行中のタスクを選択し、Copilot が実行した操作を確認します。次のような操作が含まれる場合があります。
   - リポジトリからコードをチェックアウトする。
   - Copilot の作業環境を作成する。
   - MCP server を設定する。
   - 割り当てられたタスクを完了するために複数の手順を実行する。

> [!NOTE]
> 表示される手順は、Copilot の作業状態や実行方法によって異なります。

4. 右側に表示される pull request パネルも確認します。pull request と変更されたファイルを確認して、作業をさらに監視できます。

## cloud agent の方向を示す

実行中のタスクを確認したので、Copilot がハイ コントラスト モードに取り組んでいる間にライト モードも追加するよう依頼します。

1. ハイ コントラスト モードの追加に関連するセッションを選択します。正確なタイトルは Copilot が付けた名前と作業の状態によって異なります。

    ![ミッション コントロールのアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了し、作業が始まったことを示すまで、数分間セッションを確認します。次のようなメッセージが表示されたら作業が始まっています。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![agents ページの cloud agent タスク。作業中のセッションをガイドするダイアログを強調表示](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを確認し、作業に取り込む様子を確認します。

## Copilot に作業を任せる

前と同じように、Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で作業中の手順を完了してから、新しい依頼を作業に反映します。

今回も数分かかります。これまでに学んだことや確認したことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視してガイドする中心となる、Copilot エージェント ページを確認しました。ミッション コントロールから、進行中および完了したタスクの追跡、実行内容のレビュー、進行中のタスクへの追加指示や範囲の変更ができます。

次の内容を確認しました。

- ミッション コントロールとエージェント ページを使って cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能を実装している間に、[Copilot が作成した pull request を反復して改善します][next-lesson]。

## リソース

- [Copilot エージェント ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復して改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
