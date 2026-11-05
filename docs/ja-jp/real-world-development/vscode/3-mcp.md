---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェントモード][previous-lesson] · [次のレッスン: カスタムエージェント →][next-lesson]

エージェントモードを使ってフィルタリング機能を実装しました。プルリクエストを作成する前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続し、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次の操作を行います。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot に Playwright MCP サーバーを使わせ、ブラウザーでフィルタリング機能を手動テストします。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェントモードは、エディターの外部にあるものを利用できると、さらに便利になります。Copilot が外部ツールを使うときには Model Context Protocol (MCP) を利用します。これはエージェントが外部のツールやサービスと通信するための標準的な方法です。

![エージェントモードがコンテキスト、LLM、ツール (MCP サーバーや VS Code 拡張機能が提供するツールを含む) とどのように連携するかを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部ツールやサービスと通信できます。MCP によって、エージェントはリソースを使って最新情報にアクセスしたり、ツールを使って利用者に代わって操作を行ったりできます。

エージェントは、AI と外部ツールやサービスをつなぐ MCP サーバーを通じて、ツールやリソースにアクセスします。MCP サーバーは、既存の API や npm パッケージなどのローカルツールと AI エージェントの間で通信を管理します。MCP サーバーごとに、エージェントがアクセスできるツールやリソースのセットが異なります。

よく使われている MCP サーバーの例を紹介します。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API にアクセスできます。リポジトリの新規作成や更新、Issue とプルリクエストの管理などをエージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザー自動化機能を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などをエージェントが実行できます。

ほかにもさまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub がホストする [MCP レジストリ](https://github.com/mcp)で、エコシステム内のサーバーを探してプロジェクトに貢献できます。

> [!CAUTION]
> セキュリティ上、MCP サーバーはほかの依存関係と同じように扱ってください。使用前にソースコードを確認し、発行元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーだけを使い、機密情報や重要な操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルで、この VS Code ワークスペースから利用できる MCP サーバーを設定します。

1. codespace で `.vscode/mcp.json` を開きます。
2. 次のような `github` サーバーがすでに設定されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

`github` の設定はプロジェクトテンプレートに含まれています。そのため、前の演習では Copilot がバックログの Issue を読み取れました。この設定は[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルにインストールする必要はありません。VS Code は HTTP 経由で接続し、Copilot が初めてツールを使用するときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に、2つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が操作できるブラウザーを利用できます。今回の機能テストに最適です。

1. `.vscode/mcp.json` で `github` と並べて `playwright` を追加し、ファイルを次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーと異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行して起動します。`--headless` フラグを指定すると画面を表示せずにブラウザーを実行できます。デスクトップ環境がない codespace ではこの設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトではエンドツーエンドテストに Playwright を使うため、ブラウザーはすでにインストールされていることがほとんどです。Copilot からブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行して再度試すよう依頼してください。

## サーバーを起動して信頼する

VS Code は必要に応じて MCP サーバーを起動します。Copilot が初めてサーバーのツールを必要としたときに、VS Code がサーバーを起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` の上に **Start** が表示されます。選択してサーバーを起動し、設定が正しいことを確認します。
2. VS Code からサーバーを信頼するか確認されたら、設定を確認して、サーバーを起動できるように信頼します。

手動で起動する必要はありません。ここで起動を省略すると、次の手順で Copilot が初めてツールを使う際に VS Code が起動し、信頼の確認を表示します。

## フィルタリング機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作する代わりに、エージェントがページを開き、移動し、フィルターを適用し、結果を確認して要約します。会話を中断せずに機能を検証できます。

Playwright MCP サーバーはスクリーンショットではなく、ページの[アクセシビリティツリー][playwright-mcp-server]を使います。つまり、エージェントは支援技術と同じように、ボタン、リンク、リスト項目などのラベル付き要素を認識します。そのため、基本的な機能テストを簡単なアクセシビリティ確認にも活用できます。

サーバーを接続してアプリを起動した状態で、次のプロンプトを Copilot に送信してフィルタリング機能をテストします。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバーからブラウザーを起動し、手順を実行して結果を報告します。報告内容を Issue の受け入れ条件と照らし合わせます。問題がある場合は、プルリクエストを作成する前に質問を追加するか、コードを修正するよう依頼してください。

> [!NOTE]
> テストするには、アプリが `http://localhost:4321` で起動している必要があります。開発サーバーを停止している場合は、プロンプトを送る前にもう一度起動してください。Copilot が Playwright MCP サーバーを初めて使うときにブラウザーをダウンロードする場合があります。ブラウザーが見つからないと報告された場合は、`npx playwright install chromium` を実行して再試行するよう依頼してください。

[playwright-mcp-server]: https://github.com/microsoft/playwright-mcp

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたら、チームのレビュー用にプルリクエスト (PR) を作成します。まず `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot による変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを選択してコミットメッセージを生成します。

   ![変更内容を表示する Source Control パネル](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

github.com や GitHub コマンドラインインターフェイス (CLI) など、プルリクエストの作成方法はいくつかあります。ここでは GitHub Copilot に作成を依頼します。Copilot は関連する Issue を見つけ、PR と関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント一覧で **Agent** が選択されていることを確認します。Copilot が GitHub ツールを使用できます。
3. 次のプロンプトを送信して、PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、Copilot が情報を集めて操作を実行できるようにします。GitHub ツールの初回使用時に GitHub の認証を求められた場合は、画面の指示に従います。
5. Copilot が Issue を検索し、該当するものを見つけて PR を作成することを確認します。
6. Copilot が生成したリンクを選択してプルリクエストを確認します。ただし、まだ**マージしないでください**。

## まとめと次のステップ

この演習では、次の操作を行いました。

- Model Context Protocol (MCP) と、MCP サーバーで Copilot にツールを追加する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- MCP サーバーを使ってブラウザーでフィルタリング機能を手動テストしました。
- ブランチを公開し、フィルタリング作業のプルリクエストを作成しました。

MCP サーバーを使って機能をテストしましたが、Copilot を拡張する方法は MCP だけではありません。次は[カスタムエージェント][next-lesson]を作成し、アクセシビリティレビューなど、対象を絞ったタスクを効率化します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
