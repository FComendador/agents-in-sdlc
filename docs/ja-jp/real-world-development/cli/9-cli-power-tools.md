---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、クラウドエージェントへの任意の委任に使うコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

他の CLI ツールと同様に、GitHub Copilot CLI には多くのスラッシュコマンドが用意されています。高度な機能、内部情報、追加の構成オプションを利用できます。これまでに `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` を使いました。ここでは、他の便利なコマンドも確認します。

このレッスンでは、次のことを行います。

- `/context` と `/compact` を使い、Copilot が会話コンテキストを管理する方法を確認します。
- `/model` を使い、利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、クラウドエージェントへの委任に使える任意のコマンドを確認します。

## シナリオ

CLI の中心的なワークフローは完了しました。ここでは、コンテキスト管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、追加機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクに取り組むと、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動で圧縮します。また、スラッシュコマンドで状態を確認したり、自分で圧縮したりできます。

1. Codespace に戻ります。Copilot CLI が開いていない場合は、リポジトリのルートから起動します。
2. 次を入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム指示、ツール、メッセージ、空き領域にコンテキストがどう配分されているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して結果を比較します。会話がすでに短い場合、大きな違いはないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動でコンテキストを圧縮します。圧縮のタイミングを選びたい場合は `/compact` を使います。無関係なタスクに切り替えて新しい会話を始める場合は、`/clear` または `/new` を使います。

## モデルを選択する

モデルによって得意分野が異なり、開発者によって好みも異なります。Copilot CLI では利用可能なモデルを一覧表示し、使うモデルを選択できます。

1. 次を入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況の情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選ぶか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで協力し、学んだことを共有すると、全員が AI ツールをより活用できます。`/share` コマンドを使うと、セッションを Markdown または HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンにおける `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合う宛先を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を送信または公開する前に確認します。セッションのエクスポートにはプロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を意図していないリポジトリや会話の内容は公開しないでください。

## 任意: 作業の拡大または委任

ワークショップの中心部分は完了しました。Copilot CLI には大きなタスクに使えるコマンドもあります。

- `/fleet` は独立したサブタスクをサブエージェントに分割し、並行して実行できます。
- `/worktree` は別のタスク用に独立した Git worktree を作成できます。
- `/delegate` は Copilot cloud agent にタスクを送り、非同期で処理して pull request を開くことができます。

これらのコマンドは追加の worktree やリモート作業を発生させることがあるため、任意です。範囲を明確にした新しいタスクから始め、通常のワークフローで結果を確認してください。非同期エージェント作業をさらに詳しく知りたい場合は、[クラウドエージェントのワークショップ][cloud-workshop]に進みます。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使って設定を行い、セッションを共有し、内部の動作を確認しました。このレッスンでは、次のことを行いました。

- `/context` と `/compact` で Copilot の会話コンテキスト管理を確認しました。
- `/model` で利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、クラウドエージェントへの任意の委任に使えるコマンドを確認しました。

Copilot CLI には他にも多くのスラッシュコマンドがあります。最後に、[学んだことと次のステップを確認します][next-lesson]。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
