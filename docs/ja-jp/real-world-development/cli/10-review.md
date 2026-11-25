---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つの pull request、再利用可能なカスタマイズ、追加リソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

Tailspin Toys の一連のワークフローを通じて GitHub Copilot CLI を使いました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールし、プロジェクトを確認して既存のフィルタリング issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認し、最初の pull request (PR) を手動でマージしました。
- フィルタリングの issue から始め、Plan モードで方針を定め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルタリングを確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーの根拠を評価しました。
- フィルタリングの変更全体をレビューし、フィルタリング PR に対して Agent Merge を承認しました。
- コンテキスト、モデル、共有、クラウドへの任意の委任に使うスラッシュコマンドを確認しました。

## 提出した内容

ワークショップでは、更新済みの `main` からそれぞれ作成したブランチで、2つの PR を作成します。

1. **星評価:** 既存の `starRating` と未評価状態をゲームカードに表示します。
2. **フィルタリングと品質ワークフロー:** フィルタリングを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して関連テストを含めます。

フィルタリングを計画してから PR を開くまで、同じ会話とブランチを使用しました。ワークショップを簡略化するため、この作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分でのブラウザー確認、MCP を通じた Copilot のブラウザー操作など、複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA は PR の前に、要件とテスト範囲のレビューをこれらの結果とまとめました。

追加するテストは実際の不足を補うものであるべきです。QA 実行で新しいテストが不要と判断されることもあります。マージを承認する前にコードと根拠をレビューし、変更後は影響する根拠を更新してください。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整してください。指示は標準を定め、スキルは繰り返し可能なタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。構築前に方針を検討するには **Plan**、範囲を絞った変更に関与し続けるには **Interactive**、範囲を明確にしたタスクには **Autopilot** を使います。定型的な編集には高速なモデル、複雑な作業にはより高性能なモデルを選択します。

コンテキストはインフラと同じくらい重要です。何を、なぜ、どのように作るかを明確に伝えると、出力が大きく変わります。

## さらに確認する

中心的なワークフローは完了しました。Copilot CLI には他にも確認する価値のある機能があります。

- `/review` でコードレビューエージェントに変更を分析してもらいます。
- `/rubber-duck` で問題を話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、使い続けることが上達への近道です。プロダクションコードや個人のプロジェクト、何年も作ろうと思いながら手をつけていない小さなアプリで使ってみましょう。学んだことをチームと共有し、互いの経験から学んでください。そして、引き続きドキュメントを確認しましょう。

ターミナルから Tailspin Toys を拡張したい場合は、任意の [Foundry Backer Concierge シリーズ][foundry]に進みます。他の環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認します。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI で pull request を管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
