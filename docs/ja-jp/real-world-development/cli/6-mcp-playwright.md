---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または設定し、ブラウザーでフィルタリング機能を試します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

これまでに説明したように、コードを書くことはコードだけにとどまりません。データや外部サービスを使ったり、Copilot に追加の自動化機能を提供したりする必要があります。Model Context Protocol (MCP) サーバーを使うと、Copilot は CLI に組み込まれている機能を超えて、他のツールやサービスにアクセスできます。

このレッスンでは、次のことを行います。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどう使うかを理解します。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、フィルタリング機能を試します。

## シナリオ

ユニットテストやエンドツーエンドテストは重要ですが、ユーザーインターフェイス (UI) の変更を検証するには、実際に操作する必要があります。Copilot が人間のユーザーのように作業中の Web サイトを使えるようにし、さらに自動化して、変更が期待どおりに動作する確信を高めたいと考えています。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部のツールやサービスとリアルタイムに通信できます。そのため、最新の情報にアクセスしたり、ユーザーに代わって操作を実行したりできます。

ツールやリソースは MCP サーバー経由で利用します。サーバーは AI エージェントと外部のツールやサービスの橋渡しをします。各サーバーは、エージェントにそれぞれ異なるツールとリソースを提供します。

よく使われる MCP サーバーは次のとおりです。

- **[GitHub MCP Server][github-mcp]**: GitHub のリポジトリ、issue、pull request を管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright によるブラウザー自動化を提供します。

他にも多くの MCP サーバーがあります。GitHub は、MCP サーバーを見つけてエコシステムに貢献しやすくするため、[MCP レジストリ][mcp-registry]をホストしています。

> [!CAUTION]
> MCP サーバーも他のプロジェクト依存関係と同様に扱ってください。利用前にソースコードを確認し、公開元を検証し、セキュリティへの影響を検討してください。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ利用できない場合は、追加して Copilot が人間のようにサイトを操作できるようにします。

> [!NOTE]
> Copilot CLI で Playwright MCP がすでに設定されている場合は、この手順を省略して次のセクションに進んでください。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバーを追加するダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力し、<kbd>Tab</kbd> を押します。
4. <kbd>Enter</kbd> を押してサーバーの種類 **STDIO** を確定し、<kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次を貼り付けます。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを終了します。

## Playwright で機能を試すよう Copilot に依頼する

手動で機能が期待どおりに動作することを確認しました。次に、Copilot で利用できる MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot が通常 MCP サーバーを自動で判断するため、明示的に使うよう指示する必要はありません。ただし、使用するサーバーがわかっている場合は、指定すると結果の一貫性が高まり、トークンを節約できることがあります。

2. Copilot がブラウザーで機能を確認する際に表示される手順を確認します。
3. レポートを読み、すべてが期待どおりに動作したことを確認します。

Copilot はサーバーを起動し、Playwright でサイトを操作してからサーバーを停止し、レポートを返します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を試しました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) とは何か、Copilot CLI でどう使うかを学びました。
- Playwright MCP サーバーがまだ利用できない場合は追加しました。
- エージェントにブラウザーを操作させ、フィルタリング機能を試しました。

次は、[カスタム QA エージェントを作成し][next-lesson]、スキルとブラウザーツールを組み合わせた専門的な役割を構成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP サーバー][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
