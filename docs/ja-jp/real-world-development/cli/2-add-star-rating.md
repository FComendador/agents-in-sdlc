---
title: "レッスン 2 - 星評価を追加する: 小さな改善"
description: "Copilot CLI でゲームカードに評価を表示し、転送されたブラウザーで確認して、最初の pull request をマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次は、プロジェクトに最初の変更を加えます。変更は小さなものです。ゲームデータにはすでに評価がありますが、ホームページのゲームカードには表示されていません。エージェントに既存の評価をカードに表示するよう依頼し、変更を確認して、最初の pull request としてマージします。

このレッスンでは、次のことを行います。

- 機能に焦点を当てた Copilot の会話をブランチから開始します。
- エージェントに小さなプロジェクト変更を依頼します。
- `/diff` で変更を確認します。
- アプリを起動し、転送されたブラウザーで変更を確認します。
- 最初の pull request を作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価があり、詳細ページにはすでに表示されています。しかし、ホームページのカードに表示されるのはタイトル、カテゴリ、出版社、説明だけです。まず、各カードに既存の評価を表示するようエージェントに依頼します。これは範囲の小さい変更で、最初のセッションに適しています。

## 会話の構成

**会話**は、Copilot CLI でタスクに取り組む作業スペースです。Copilot アプリとは異なり、通常の CLI 会話では、専用の worktree ではなく、現在のターミナルにある Git リポジトリとブランチを使用します。保存した会話に戻ることはできますが、ファイルとブランチは通常どおりディスク上の Git の状態です。

会話では、プロンプトとエージェントの応答、ファイルを探索・編集する際のツールのアクティビティ、`/diff` で確認できる変更の3つを目にします。

## 会話を開始して変更を依頼する

機能を実装するため、新しい会話を開始します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. Copilot CLI が起動していない場合は、次を実行します。

   ```bash
   copilot --yolo
   ```

4. `/new` スラッシュコマンドを入力し、<kbd>Enter</kbd> を押して新しいセッションを開始します。
5. 次のプロンプトで変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけ、必要なコードを作成します。Copilot CLI を使って機能を追加できました。

## 差分を確認する

マージする前に、たとえ小さな変更でも、AI が生成した変更をすべて確認します。Copilot CLI で変更を調べましょう。

1. `/diff` と入力し、変更されたすべてのファイルを確認します。
2. 評価が存在する場合は数値が表示され、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. どちらの状態もテストで確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. 確認が終わったら、<kbd>Esc</kbd> を押して diff ビューを終了します。

> [!NOTE]
> 生成 AI ツールは決定的ではなく確率的に動作するため、生成されるコードは毎回異なることがあります。同一の実装を期待するのではなく、動作を確認してください。

## 変更を検証する

コードを読んで動作すると決めつけるだけでは不十分です。Copilot にサイトを起動してもらい、Codespaces で転送されたブラウザーから更新されたユーザーインターフェイス (UI) を確認します。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. ゲームカードに5段階の評価が表示されることを確認します。
4. Copilot に戻り、起動したサーバーを停止するよう依頼します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初の pull request を作成してマージする

機能を作成できました。次に、新しいコードをプロジェクトにマージするため、pull request (PR) を作成します。

1. 既定のエージェントに変更をコミットするよう依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` と入力します。Copilot CLI は pull request の作成時に既存のコミットをプッシュし、URL を表示します。
3. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、Copilot CLI に表示された URL を選択して PR を開きます。
4. 変更されたファイルとチェック結果を確認します。
5. 問題がなければ **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

おめでとうございます。GitHub Copilot CLI を使って最初の変更を公開しました。このレッスンでは、次のことを行いました。

- ブランチから機能に焦点を当てた Copilot の会話を開始しました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更を確認しました。
- アプリを実行し、転送されたブラウザーで評価を確認しました。
- 最初の pull request を作成してマージしました。

次は、[フィルタリングの issue をもとに Plan モードと Autopilot モードを使って][next-lesson]、より大きな機能を開発します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドのリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
