---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム命令][previous-lesson] |
|:--|

多くの組織では、解決されていないセキュリティ問題、更新が必要なレガシーコード、実装の時間がなくバックログに残った機能要望など、技術的負債に直面しています。GitHub Copilot の cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。手間のかかるタスクを任せることで、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを学びます。

- コード生成環境をカスタマイズする方法
- 安全に操作する方法
- 範囲を明確にした Issue が重要である理由
- Issue を Copilot に割り当てる方法

## シナリオ

Tailspin Toys では、解決したい技術的負債があります。最初の Web サイトを作成した委託先がドキュメントを十分に整備していませんでした。まず、アプリケーション内のエクスポートされた関数すべてに TSDoc のドキュメントコメントを追加します。

さらに、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する関連ゲームを表示して、利用者がほかのゲームも見られるようにします。完成度の高い実装ではなく、ユーザー体験の受け入れテストに使えるものを求めています。このタスクは現在ブロッカーですが、優先度がさらに高い別の Issue もあります。

どちらも優先度が下がりやすいタスクです。Copilot cloud agent に割り当てれば、開発者が別の作業を進める間に非同期で処理できます。後で結果をレビューし、期待どおりか確認できます。

## GitHub Copilot cloud agent について

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同様にバックグラウンドで作業できます。たとえば、[GitHub Issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法があります。割り当てると Copilot は進行状況を追跡するドラフト PR を作成し、環境を設定して作業を開始します。作業中や完了後にセッションを確認できます。提案された解決策のレビュー準備ができると、プルリクエストで通知されます。

## 範囲を明確にした指示の重要性

GitHub Copilot に魔法はありません。考える作業を省略できるわけではありません。一見簡単そうな操作にも、詳細を確認すると複雑な部分があります。[Copilot cloud agent に依頼するタスクの範囲を慎重に決めてください][cloud-agent-best-practices]。AI ペアプログラマーとして扱い、段階を踏んで学習、実験、調整します。生成 AI が加わっても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム命令

前の演習では、Copilot をガイドするカスタム命令を学びました。まだ `.github/copilot-instructions.md` を見ていない場合は、この機会に確認してください。cloud agent に作業を依頼する前に、リポジトリに用意された命令ファイルを読み取り専用で確認して、後でその影響を確認できるようにします。

リポジトリの GitHub Web UI、または起動済みの codespace で次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript の規約と TSDoc ドキュメントコメントに関する内容を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターで `**/*.test.ts` に命令の対象が限定されていることを確認します。

次のセクションで `Code lacks documentation` Issue を cloud agent に割り当てるとき、作成された PR の TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。これらの命令ファイルの影響は、後の演習で PR をレビューするときにも確認します。

## Copilot cloud agent の開発環境をセットアップする

開発者がコードを作成するときと同じように、Copilot にタスクを割り当てる際にも、開発環境やセットアップスクリプトが必要です。cloud agent もソフトウェアエンジニアのように作業します。

cloud agent は作業環境に [GitHub Actions][github-actions] を使用します。作業開始前に実行する[セットアップワークフロー][setup-workflow]を `.github/workflows/copilot-setup-steps.yml` に作成して、必要な開発ツールや依存関係を利用できるように設定できます。このラボでは円滑に進められるよう、あらかじめ設定されています。単一の Astro アプリのために、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションと初期データ投入済みの SQLite データベースを利用できるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # 手順に必要な最小限の権限を設定します。Copilot は操作用の独自トークンを受け取ります。
    permissions:
      # セットアップ手順の一部としてリポジトリをクローンする場合 (依存関係のインストールなど) は `contents: read` が必要です。クローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ローカル SQLite データベースをマイグレーションし、初期データを投入します (ビルドとテスト用)。
      - name: Set up the database
        run: npm run db:setup
```

このファイルは一般的な GitHub ワークフローと同じですが、重要な点がいくつかあります。

- `copilot-setup-steps` というジョブが1つあります。このジョブは、Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

多くのプロジェクトでは、ドキュメントが古いか、まったく用意されていません。このような技術的負債は対応されないままになり、開発効率や保守性を下げ、新しい開発者が参加する際の負担になります。Copilot はドキュメントの作成を得意とするため、cloud agent に割り当てるのに適したタスクです。バックグラウンドでドキュメントを生成してもらい、後の演習で結果をレビューします。

1. 新しいブラウザータブで github.com のリポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` などです。PR が一覧に表示されない場合は、数分待ってからブラウザーを更新します。
12. 数分後、Copilot がタスクリストを作成したことを確認します。

> [!NOTE]
> Copilot が PR にタスクリストを表示するまで数分かかることがあります。Copilot は先ほど説明したワークフローを実行して環境を作り、プロジェクトを分析して、最適な方法を検討しています。

13. タスクリストと、Copilot が実行する作業を確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が Issue の作業を開始したことを確認します。
15. **View session** を選択します。

    ![Copilot セッションの表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 状態の更新が表示されない場合は、ブラウザーの画面を更新してください。

16. ライブセッションをスクロールして、Copilot の作業を確認します。コードを調べて状態を把握し、計画を考え、コードを作成する様子が表示されます。

この作業には数分かかることがあります。Copilot cloud agent の主な目的の1つは、非同期で作業している間、開発者が別のタスクに集中できるようにすることです。この機能を活用するため、別のタスクも cloud agent に割り当て、その間にアプリケーションの機能を追加します。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の1つは、作業を分けて、開発者が別のタスクに取り組めることです。デザインチーム向けに関連ゲームの表示を追加する作業が短時間で終わるとしても、その時間をほかのタスクに使えます。cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次のテキストを入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。

    ![Issue に Copilot を割り当てる画面](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

すぐに Issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことを確認できます。

![Issue に Copilot が取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼に取り組んでいます。Copilot cloud agent はソフトウェアエンジニアのように作業するため、常に監視する必要はありません。完了後に結果をレビューできます。続けてカスタムエージェントの作成と使用を確認しましょう。

## まとめと次のステップ

このレッスンでは、[GitHub Copilot cloud agent][copilot-agents]について学びました。cloud agent に Issue を割り当てて、非同期で作業させることができます。技術的負債への対応、新しい機能の作成、別のフレームワークへのコード移行などに活用できます。

次の内容を学びました。

- コード生成環境をカスタマイズする
- 安全に操作する
- 範囲を明確にした Issue が重要である理由
- Issue を Copilot に割り当てる

cloud agent がバックグラウンドで作業している間に、次はカスタムエージェントを作成して使用します。[Copilot cloud agent は MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで学んだカスタム命令にも対応しています。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
