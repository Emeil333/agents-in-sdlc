---
title: "レッスン 6 - Playwright MCP で機能を検証する"
description: "Copilot CLI で Playwright MCP を確認または構成し、ブラウザーで絞り込み機能を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

コードを書くこと以外にも、データや外部サービスとの連携、Copilot が使える自動化機能の追加などが必要です。MCP サーバーを使うと、Copilot は CLI に標準で組み込まれていないツールやサービスも利用できます。

このレッスンでは、次の操作を行います。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びます。
- Playwright MCP サーバーがまだ利用できない場合は追加します。
- エージェントにブラウザーを操作させ、絞り込み機能を確認します。

## シナリオ

ユニットテストとエンドツーエンドテストは重要ですが、UI の変更を検証するには、実際に UI を操作する必要があります。利用者と同じように Web サイトを操作する機能を Copilot に提供し、変更が期待どおりに動くことへの信頼性を高めます。

## Model Context Protocol (MCP) とは

[Model Context Protocol (MCP)][mcp-blog-post] を使うと、AI エージェントは外部ツールやサービスと通信できます。MCP によって、エージェントは最新情報にアクセスしたり、利用者に代わって操作を行ったりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを通じてアクセスします。MCP サーバーごとに、エージェントがアクセスできるツールやリソースのセットが異なります。

よく使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server][github-mcp]**: GitHub リポジトリ、Issue、プルリクエストを管理する API にアクセスできます。
- **[Playwright MCP Server][playwright-mcp-server]**: Playwright を使ったブラウザー自動化機能を提供します。

ほかにもさまざまな MCP サーバーがあります。GitHub がホストする [MCP レジストリ][mcp-registry]で、エコシステム内のサーバーを探してプロジェクトに貢献できます。

> [!CAUTION]
> MCP サーバーはほかの依存関係と同じように扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。

## Playwright MCP サーバーを追加する

Playwright MCP サーバーがまだ使えない場合は、Copilot が利用者と同じように Web サイトを操作できるよう、サーバーを追加します。

> [!NOTE]
> Copilot CLI ですでに Playwright MCP が設定されている場合は、このセットアップを省略して次のセクションに進みます。

1. Codespace に戻ります。
2. Copilot CLI で次のコマンドを入力し、MCP サーバー追加ダイアログを開きます。

   ```plaintext
   /mcp add
   ```

3. 名前として `playwright` を入力して、<kbd>Tab</kbd> を押します。
4. **STDIO** サーバーが選択されていることを確認して <kbd>Enter</kbd> を押し、続けて <kbd>Tab</kbd> を押します。
5. **Command** ダイアログに次の内容を入力します。

   ```plaintext
   npx -y @playwright/mcp@latest --headless --no-sandbox
   ```

6. <kbd>Control</kbd>+<kbd>S</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>S</kbd> (Windows/Linux) を押して、新しい MCP サーバーを保存します。
7. <kbd>Esc</kbd> を押して MCP ダイアログを閉じます。

## Playwright を使って Copilot に機能を確認させる

前のレッスンでは、機能が期待どおりに動くことを手動で確認しました。次は Copilot で利用できる Playwright MCP サーバーを使って同じ確認を行います。

1. 次のプロンプトを使い、Playwright MCP サーバーで機能を検証するよう Copilot に依頼します。

   ```plaintext
   Start the app and use Playwright MCP to check filtering against the issue and our plan. Tell me what works and what doesn't, without making changes. Stop the server you started when you're done.
   ```

> [!NOTE]
> Copilot は通常、自動的に MCP サーバーを見つけるため、明示的に使うよう指示する必要はありません。ただし、使用するツールが分かっている場合はプロンプトで示すと、より一貫した結果になり、トークンを節約できます。

2. ブラウザーで機能の動作を確認するため、Copilot が実行する各手順を確認します。
3. Copilot の報告を読み、すべてが期待どおりに動作することを確認します。

Copilot はサーバーを起動し、Playwright で Web サイトを操作して、サーバーを停止してから結果を報告します。

## まとめと次のステップ

Copilot CLI から Playwright MCP サーバーを使い、実際のブラウザーで機能を確認できました。このレッスンでは、次のことを行いました。

- Model Context Protocol (MCP) と Copilot CLI での使い方を学びました。
- Playwright MCP サーバーが未設定の場合に追加しました。
- エージェントにブラウザーを操作させ、絞り込み機能を確認しました。

次は[QA カスタムエージェントを作成][next-lesson]し、スキルとブラウザーツールを専門エージェントとして組み合わせます。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [Copilot CLI に MCP サーバーを追加する][add-mcp]

[previous-lesson]: ../5-agent-skills/
[next-lesson]: ../7-qa-agent/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp
[github-mcp]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[add-mcp]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-mcp-servers
