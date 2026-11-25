---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "フィルタリング、指示、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

フィルタリングの実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて1つのブランチに保存されています。まとめて確認し、pull request を開きましょう。星評価の pull request (PR) は自分でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分割します。ワークショップを簡略化するため、フィルタリングと品質に関する作業を1つの会話とブランチで進め、1つの PR にまとめました。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、PR のライフサイクルをどう自動化するかを学びます。
- 機能の変更全体と検証の根拠を確認します。
- フィルタリングの PR を作成します。
- レビュー後に Agent Merge を有効化し、PR がマージされたことを確認します。

## シナリオ

フィルタリングのワークフローを通じて、Copilot で機能を計画、実装、検証しました。Tailspin Toys は、マージを許可する権限は開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は pull request を取り込むために必要な残りの作業を自動化します。有効にすると、Copilot は PR を妨げているものに対処します。失敗した継続的インテグレーション (CI) チェックの修正、レビューコメントへの対応、必要な場合のリベースを行い、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることができます。有効化する前に内容をレビューし、マージ可能だと判断してください。

## Agent Merge で PR を管理する

コードの作成が終わったので、まとめてレビューし、PR を作成して、残りのプロセスを Agent Merge に任せましょう。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントのダイアログを開きます。
3. オプション一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功するとマージを実行します。
7. Copilot が `"PR #14 was squash-merged successfully."` のようなメッセージを表示したことを確認します。

> [!IMPORTANT]
> Agent Merge は必須承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は報告された理由を読み、リポジトリで許可されている場合はレビュー済みのマージを手動で完了します。

## まとめと次のステップ

コードの生成、テストと検証、pull request のプロセスなど、開発作業の複数の部分を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge とは何か、PR のライフサイクルをどう自動化するかを学びました。
- 機能の変更全体と検証の根拠を確認しました。
- フィルタリングの PR を作成しました。
- レビュー後に Agent Merge を有効化し、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、クラウドへの任意の委任に使える[便利な Copilot CLI スラッシュコマンド][next-lesson]を確認します。

## リソース

- [Copilot CLI で pull request を管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
