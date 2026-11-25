---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスを改善して機能を追加する複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザイン チームが改善を重ねられる関連ゲーム機能の作成、ハイ コントラスト モードとライト モードを含むアクセシビリティ機能の実装を依頼しました。コードの変更を確認し、必要に応じて Copilot にフィードバックを送って改善しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに従います。チーム全体のレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能に関する GitHub Copilot の提案を確認しましょう。

## GitHub Copilot cloud agent のセキュリティ

Copilot cloud agent は監督なしで非同期にタスクを実行するため、安全性を保つための制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、コードの作業に使用するブランチに限って書き込みアクセスを持ちます。
- cloud agent は GitHub Actions 内で、独立した一時的な環境を作成して実行します。
- GitHub Actions のワークフローを実行するには、人間による承認が必要です。
- MCP server を含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

最初に、GitHub Copilot cloud agent が作成した、コードにドキュメントを追加する最初の pull request (PR) をレビューします。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するとき、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。これは**想定どおり**です。外部 MCP server への呼び出しを含む外部リソースへのアクセスは、既定で制限されています。必要であれば、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して一覧を開きます。
3. **Add missing documentation** などのタイトルが付いた PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。Copilot が作業を完了するまで待ちます。数分かかる場合があるため、休憩したり、これまでに学んだことを振り返ったりしてください。

4. PR の準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しい TSDoc コメントやその他のドキュメントが含まれる、更新されたコードを確認します。具体的な変更内容は異なる場合があります。

   確認時には、TSDoc コメント、TypeScript の規約、ファイル ヘッダーを探します。これらは演習 2 の冒頭で確認したカスタム指示ファイルによるものです。

6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す通知を確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

PR で Copilot と作業するのは一方通行ではありません。チーム メンバーに依頼するのと同じように、PR のコメントやコードへのインライン コメントで Copilot にメンションできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、指定の依頼文はありません。たとえば、次のような変更を依頼できます。

- 各コード ファイルの先頭に、内容を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc コメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 生成されたドキュメントの変更を依頼するコメントを追加し、ほかのユーザーと同じように **@copilot** をメンションします。上記の例またはコードベースに必要な別のドキュメントを依頼してください。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が変更用の新しいセッションを開始します。
3. **Back to pull request** を選択すると PR に戻れます。

    ![pull request に戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで反復してもかまいません。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージできます。

![PR をレビュー可能な状態に変更](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue について、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
6. 承認待ちのワークフローがあることを示す通知を確認します。
7. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションにワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
9. **省略可能:** codespace でこのブランチに切り替えて、関連ゲーム機能を手動でテストできます。codespace に移動してターミナルを開き、次のコマンドを実行します。`<branch-name>` は Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えてください。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。前と同じように、Copilot cloud agent に更新を依頼しながら反復できます。たとえば、表示する関連ゲームの数を調整したり、カスタム指示の更新**前**に割り当てた作業だったことを踏まえて、コメント ヘッダーや TSDoc コメントの追加を依頼したりできます。更新を依頼するには、**Conversation** タブにコメントを追加します。Copilot がコメントを確認し、新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティのカスタム エージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 のミッション コントロールから追加を依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR に **[WIP]** が表示されます。作業が完了するまで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択し、生成されたコードをレビューします。
5. 実装を確認し、特に次の点に注目します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保持するためのローカル ストレージの使用。
   - ハイ コントラスト モードとライト モードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード ナビゲーションなど)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新をレビューして問題がなければ **Conversation** タブに戻り、下にスクロールします。
7. 承認待ちのワークフローがあることを示す通知を確認します。
8. ワークフローが承認待ちの場合は **Approve and run workflows** を選択します。

   ![ワークフローを承認して実行](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションにワークフローがキューに追加されます。問題がなければ、単一の Astro アプリのプロジェクト チェックが成功します。完了まで数分かかる場合があります。
10. **省略可能:** codespace でこのブランチに切り替えて、アクセシビリティ機能を手動でテストできます。ターミナルを開き、`<branch-name>` を Copilot が作成したブランチ名に置き換えて、次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みした後も設定が保持されることを確認します。

アクセシビリティのカスタム エージェントは、アクセシビリティのベスト プラクティスに沿ってこれらの機能を実装するよう Copilot をガイドしました。アクセシビリティ上の問題や改善点があれば、前の PR と同様にコメントで **@copilot** にメンションして更新を依頼できます。

## 省略可能な演習 — 委任を続ける

cloud agent には、実際のバックログ項目を割り当てて別の作業に集中すると効果的です。練習として、リポジトリに Issue をいくつか追加し、Copilot に割り当てます。たとえば、次のような作業があります。

- ゲーム詳細ページに支援者向けの関心フォームを追加する。
- ゲーム一覧ページにページネーションを実装する。
- データ アクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

cloud agent の環境を完了しました。このレッスンでは、次の操作を行いました。

- **リポジトリに含まれるカスタム指示ファイルを確認しました。** 後で cloud agent の出力に及ぼす影響を確認できます。
- **GitHub Copilot cloud agent に Issue を割り当てました。** 環境を設定し、計画して、非同期に作業する様子を確認しました。
- **アクセシビリティのカスタム エージェントを作成して使用しました。** ハイ コントラストとライト モードの切り替えを追加しました。
- **Copilot エージェント ページをミッション コントロールとして使用しました。** アクセシビリティ セッションを監視し、作業中に方向を示しました。
- **cloud agent の pull request をレビューして反復しました。** `@copilot` に変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

cloud agent の環境を完了しました。さらに確認したい場合は、ほかの環境も今回の内容を補完します。

- 🖥️ **[VS Code 環境](../../vscode/)** — IDE (統合開発環境) から Copilot Chat のエージェント モードと MCP 統合を確認します。
- 💻 **[CLI 環境](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、QA カスタム エージェント、Playwright MCP、Agent Merge、省略可能な `/delegate` を使い、ターミナルからレビュー済みの変更を 2 つ届けます。

自分のリポジトリでは、次のアイデアも試せます。

- リファクタリングやテスト カバレッジの Issue を cloud agent に割り当てる。
- 別の分野のカスタム エージェントを作成する。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローのベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall
[previous-lesson]: ../4-managing-agents/
