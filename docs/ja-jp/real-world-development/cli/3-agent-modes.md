---
title: "レッスン 3 - エージェントモード: Plan と Autopilot"
description: "Plan で方針を合意し、Autopilot で issue からフィルタリングを実装し、Interactive モードで結果を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初は、小さな機能をプロジェクトに追加しました。より大きな変更には、より確かなプロセスが必要です。GitHub Copilot CLI は組織のワークフローに組み込めるため、適切な方法で適切なものを開発できます。このレッスンと後続のレッスンでは、issue を起点に機能を作成し、コードが有効で期待どおりに動作することを確認して、最後にプロジェクトへマージするという、一般的なエージェント開発プロセスを進めます。

> [!NOTE]
> 同じ会話とブランチを使って、この機能開発を続けます。通常はファイルの種類ごとに別のブランチや PR を使いますが、ここでは主な概念に集中するため一部の手順を簡略化します。

このレッスンでは、次のことを行います。

- GitHub issue から新しい Copilot の会話を開始します。
- Plan モードで要件を確定します。
- Autopilot モードで機能を実装します。
- コードをレビューします。
- 転送されたブラウザーで機能を手動検証します。

後続のレッスンでは、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP 検証を追加して QA エージェントを作成し、機能の PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者はカテゴリや出版社でゲームを絞り込みたいと考えています。バックログの issue には機能が記載されていますが、カテゴリをどのように組み合わせるかなど、コードを書く前に合意すべき詳細があります。Plan でそうした判断を整理し、範囲を定めた実装を Autopilot に任せます。

## 背景

AI コーディングエージェントを開発ワークフローに加えても、基本は変わりません。むしろ、基本がいっそう重要になります。多くの開発者は、次のようなプロセスに従います。

1. 作業内容を記述した issue から始めます。
2. 作成するものの計画を立てます。
3. コードを記述し、レビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. pull request (PR) を作成します。
7. コードレビューと継続的インテグレーション (CI) のチェックが完了したら、変更をマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くのプロセスはこの方法を応用したものです。

この標準的なプロセスにより、AI 生成コードが要件を満たし、手書きのコードと同じレビューを通過するようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すと、モードを切り替えられます。

- **Interactive**: エージェントと一緒に作業します。変更案を提示し、続行する前に返答を待ちます。
- **Plan**: まず計画を作成します。プロジェクトファイルは編集できません。
- **Autopilot**: コードを書き、テストを実行し、タスクが完了するまで反復する自律モードです。

最初に Plan で計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルタリングに取り組む前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は再起動します。
2. 評価機能の PR がマージ済みであることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新し、フィルタリング用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を2回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押して、すべての issue を表示します。
8. 矢印キーを使って **Allow users to filter games by category and publisher** を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトは `#7` などの番号から始まります。`#` を使うと、GitHub issue または PR をコンテキストに追加できます。

## フィルタリング機能を計画する

計画を立てることで、機能の実装やタスクを Copilot に任せる前に、アプローチを決められます。複雑なタスクには時間をかけて計画するのが適切です。Plan モードに切り替えて計画の作成を依頼します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下に **Plan** と表示されていることを確認します。
2. 先ほど追加した issue の参照の後に、次を入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べ、計画を作成するための適切なアプローチを判断します。

3. フィルタリングの動作について質問された場合は、希望に応じて回答します。正解はひとつではありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して展開します。
5. 計画をスクロールして確認します。
6. 決めた内容と異なる部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成して確認できました。次は Autopilot で実装します。

Autopilot を使うと、Copilot はタスクが完了したと判断するまで反復できます。

1. **Accept plan and build on autopilot (recommended)** または使用中のバージョンに相当するオプションを選択します。
2. プロンプトの下に **Autopilot** と表示されていることを確認します。
3. Copilot が計画に従ってコードを生成し、テストを実行する様子を確認します。

> [!NOTE]
> 計画を承認すると実装がすぐに始まる場合があります。承認前に必ず確認してください。依存関係が不足している、またはポートが競合していると Copilot が報告した場合は、チェック完了と判断する前に設定上の問題を解決します。

## 実装をレビューして検証する

他のコードと同様に、生成されたコードもマージ前にレビューします。コードを確認し、サイトを実行して動作を検証します。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示に **Plan** または **Autopilot** がないことを確認します。
2. `/diff` と入力し、フィルタリングの実装とテストを確認します。
3. 結果を issue と計画時の判断内容と比較します。
4. コードを確認したら、<kbd>Esc</kbd> を押して diff ビューを終了します。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を試す

コードが正しく見えても、実行できるとは限りません。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら、**Open in Browser** を選択します。
3. 計画した内容に従い、カテゴリ、出版社、および組み合わせのフィルターを試します。
4. リセットと結果なしの状態が issue および決定内容と一致することを確認します。
5. Codespace に戻り、起動した開発サーバーを停止するよう Copilot に依頼します。

## まとめと次のステップ

複数の会話モードを使い、機能を作成して確認しました。このレッスンでは、次のことを行いました。

- GitHub issue から新しい Copilot の会話を開始しました。
- Plan モードで要件を確定しました。
- Autopilot モードで機能を実装しました。
- コードをレビューしました。
- 転送されたブラウザーで機能を手動検証しました。

次は、[カスタム指示を使って][next-lesson]、文書化されたプラクティスに従ってコードを生成する方法を確認します。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンドのリファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
