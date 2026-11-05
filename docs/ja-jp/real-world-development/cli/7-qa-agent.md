---
title: "レッスン 7 - QA エージェントを作成して使用する"
description: "Issue の要件、quality-checks スキル、Playwright MCP を組み合わせる QA カスタムエージェントを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

`quality-checks` スキルを使って自動チェックを実行し、Playwright MCP でブラウザーから絞り込み機能を確認しました。次はこれらの機能を、明確な QA プロセスを持つカスタムエージェントにまとめます。

このレッスンでは、次の操作を行います。

- カスタムエージェントが命令、スキル、MCP ツールと連携する方法を確認します。
- 再利用可能な品質保証プロファイルを作成して確認します。
- QA エージェントを選択し、絞り込みの Issue に照らし合わせて報告をレビューします。

## シナリオ

Tailspin Toys では、プルリクエスト (PR) を作成する前に、要件、コード品質、自動チェック、テストカバレッジ、ブラウザーでの動作を一貫して確認したいと考えています。カスタムエージェントを使って QA プロセスを調整し、再利用できる報告を作成できます。

## カスタムエージェントとは

カスタムエージェントは、Markdown プロファイルで定義する Copilot の専門エージェントです。プロファイルにはエージェントの目的、命令、利用可能なツールを記述します。このワークショップでは `.github/agents/qa.agent.md` に QA の役割を定義し、Copilot CLI で選択します。

これまでに使ったカスタマイズには、それぞれ異なる役割があります。リポジトリの命令はチームの標準を説明し、`quality-checks` スキルは繰り返し実行するチェックをまとめます。Playwright MCP はブラウザーツールを提供します。QA プロファイルは、要件を評価して結果を報告するために、これらの機能を Copilot がどう使うかを示します。既存の機能を置き換えたり、別の会話を必要としたりするものではありません。

## QA プロファイルを作成する

機能の PR を作成する前に、再利用可能な QA プロファイルを Copilot に作成させます。QA が行うチェックと守るべき境界を定義します。

1. Codespace に戻り、絞り込みの会話が Interactive モードであることを確認します。
2. 既定のエージェントに、次のカスタムエージェント作成を依頼します。

   ```plaintext
   Create a custom agent named QA in .github/agents/qa.agent.md. It should check features against their issues and agreed requirements, follow the repository instructions, run the quality-checks skill, use Playwright MCP to verify behavior, and add tests when coverage is missing.

   Have it report each requirement as pass, fail, or blocked with supporting evidence. It must ask before changing implementation code, and it must not commit changes or open pull requests.

   Just create the profile for now so I can review it.
   ```

## プロファイルを確認する

新しいエージェントを使う前に、意図した QA ワークフローと境界がプロファイルに記載されていることを確認します。確認のみを依頼しているのに、エージェントが機能を変更するような事態を防げます。

1. `/diff` を入力し、`.github/agents/qa.agent.md` を開きます。
2. フロントマターを確認します。`description` は必須です。`name` は省略可能ですが、記載するとエージェントの表示名を分かりやすくできます。
3. プロファイルの命令を読みます。QA が要件を起点とし、リポジトリの命令に従い、`quality-checks` スキルを実行し、Playwright MCP を使うことを確認します。
4. QA が証拠を示し、実装コードを変更する前に許可を求め、変更をコミットしたりプルリクエストを作成したりしないことを確認します。
5. 役割や制約が不足している場合は、続行する前に既定のエージェントに修正を依頼します。

## Issue に対して QA を実行する

Copilot CLI は会話の開始時にプロジェクトエージェントを読み込みます。プロファイルを作成した後、同じ絞り込みの会話を再開して QA を選択します。Issue と計画の内容をそのまま利用できます。

1. 次のプロンプトでエージェントを有効にします。

   ```plaintext
   /agent QA
   ```

> [!NOTE]
> 新しく作成したエージェントは、利用可能なエージェント一覧にまだ表示されない場合があります。上記のコマンドで有効にできます。

2. 次のプロンプトを使い、QA エージェントに機能をレビューさせます。

   ```plaintext
   Review the filtering feature against the issue and the decisions in our plan. Is it ready for a PR?
   ```

3. QA エージェントが作業を開始します。
4. 作業が完了したら、報告内容を確認します。

## まとめと次のステップ

再利用可能な専門エージェントをワークフローに追加し、作業結果をレビューしました。このレッスンでは、次のことを行いました。

- カスタムエージェントが命令、スキル、MCP ツールと連携する方法を確認しました。
- 再利用可能な品質保証プロファイルを作成して確認しました。
- QA エージェントを選び、絞り込みの Issue に対する調査結果をレビューしました。

これで、実装、スキルの更新、QA プロファイル、テスト、検証報告の準備ができました。次は[機能 PR にまとめ、Agent Merge を使用][next-lesson]します。

## リソース

- [Copilot CLI 用のカスタムエージェントを作成する][create-agents]
- [カスタムエージェントの構成][agent-config]

[previous-lesson]: ../6-mcp-playwright/
[next-lesson]: ../8-create-pull-request/
[create-agents]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/create-custom-agents-for-cli
[agent-config]: https://docs.github.com/copilot/reference/custom-agents-configuration
