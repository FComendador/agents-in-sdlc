---
title: "演習 3 - カスタム エージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタム エージェントとは

GitHub Copilot の[カスタム エージェント][custom-agents-concept]を使うと、開発ワークフロー内の特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルでエージェントを定義すると、特定の作業をより効果的に行えるよう、Copilot に焦点を絞った指示、ベスト プラクティス、コーディング パターン、分野固有の知識を提供できます。チームの専門知識を再利用可能なエージェントとしてまとめることもできます。たとえば、[Web Content Accessibility Guidelines (WCAG)][wcag] に準拠させるアクセシビリティ エージェント、安全なコーディング プラクティスに従うセキュリティ エージェント、テスト パターンを統一するテスト エージェントなどです。

カスタム エージェントは、プロジェクトの `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` フォルダーにある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML frontmatter と、エージェントの動作、専門知識、指示を定義する Markdown プロンプトが必要です。

### カスタム エージェントとエージェント スキルの違い

カスタム エージェントと[エージェント スキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI に作業方法を伝えます。簡潔に言えば、**カスタム エージェントが作業者で、スキルがツール**です。

カスタム エージェントには独自のコンテキスト ウィンドウがあり、作業の一環としてスキルやほかのエージェントを調整します。この演習では、アクセシビリティのカスタム エージェントがアクセシビリティ ガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、pull request ワークフローのスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタム エージェントの作成に唯一の正解はありません。ほかの AI 活用と同様に、環境やシナリオに合わせてテストし、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

カスタム エージェントを使って、次の内容を確認します。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方法。

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディング プラットフォームを利用できるようにすることに取り組んでいます。最近、テキストと背景色のコントラストが不十分で、現在のダーク テーマを読みづらいという意見が寄せられました。このアクセシビリティの問題に対応するため、デザイン チームはユーザーが切り替えられるハイ コントラスト モードを求めています。

アクセシビリティは重要なので、この機能をできるだけ早く実装します。カスタム エージェントを使って機能を作成します。

## アクセシビリティのカスタム エージェントを確認する

アクセシビリティ用のカスタム エージェントはすでに用意されています。内容を確認し、Copilot のガイド方法を理解しましょう。

codespace に戻り、アクセシビリティのカスタム エージェント ファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. YAML frontmatter の `name` と `description` フィールドを確認します。

> [!CAUTION]
> カスタム エージェントには、`name` と `description` の frontmatter が必要です。

3. 続くセクションを確認し、次の内容に注目します。
   - アクセシブルな Web サイトのコード生成における主な責任。
   - アクセシビリティのベスト プラクティス。
   - HTML、CSS、JavaScript のコード例。
   - よくある問題や間違い。

## Issue を作成して割り当てる

ミッション コントロールは、環境内のエージェントを管理する中心的な場所です。Copilot cloud agent にタスクを割り当て、進捗を監視し、追加のガイダンスを提供できます。ハイ コントラスト モードの作成タスクを割り当てましょう。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側にある **Assign to Copilot** を選択して、割り当てのダイアログを開きます。
9. カスタム エージェントの一覧から **Accessibility agent** を選択します。

    ![ハイ コントラスト モード Issue にアクセシビリティ エージェントを割り当てる](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、特定のタスクや分野に特化した AI アシスタントである GitHub Copilot の[カスタム エージェント][custom-agents]を確認しました。チームの専門知識や標準を再利用可能なエージェントとしてまとめ、Copilot が特定の作業をより効果的に行えるようにガイドできます。

次の内容を確認しました。

- カスタム エージェントの定義方法。
- カスタム エージェントへのタスクの割り当て方法。

Copilot がハイ コントラスト モードを実装する間、ミッション コントロールから[エージェント セッションを監視し、方向を示します][next-lesson]。

## リソース

- [カスタム エージェントについて][custom-agents]
- [組織でカスタム エージェントを使用する準備][org-custom-agents]
- [エンタープライズでカスタム エージェントを使用する準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
