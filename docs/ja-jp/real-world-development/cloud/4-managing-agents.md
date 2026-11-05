---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー体験の改善と機能追加を目的に、GitHub Copilot cloud agent に3つのタスクを依頼しました。cloud agent は非同期で自律的に作業しますが、進行状況を監視することも大切です。

cloud agent に割り当てたタスクは、GitHub.com の[エージェントページ][agents-page]など、さまざまなツールで管理できます。このミッションコントロールでは、オープンなプルリクエスト (PR) があるエージェントのタスクを確認できます。実行内容を調べたり、進行中のセッションに追加の指示を送ったりできます。

このレッスンでは、次の操作を行います。

- エージェントページで cloud agent のタスクを監視します。
- 進行中のセッションに指示を追加して、機能を拡張します。

## シナリオ

エージェントにハイコントラストモードを依頼した後、チームはライトモードも追加したいと考えました。サイトのスタイルと切り替え機能をすでに更新しているため、同じ作業に含めることにしました。エージェントに追加の指示を送り、ライトモードとハイコントラストモードの両方を実装するようガイドします。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたタスクの現在の状況を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)を開きます。
2. メイン領域と左側の領域に表示されるタスク一覧を確認します。次のようなタスクが表示されます。
   - コードベースのドキュメントの更新
   - ゲーム変更用 API の生成
   - Web サイトへのハイコントラストモードの追加
3. 実行中のタスクを選択し、Copilot が行った作業を確認します。次のような操作が含まれる場合があります。
   - リポジトリのチェックアウト
   - Copilot が作業する環境の作成
   - MCP サーバーのセットアップ
   - タスクを完了するための各種操作

> [!NOTE]
> 表示される手順は、作業の状態や Copilot が選択した方法によって異なります。

4. 右側に表示される PR の領域も確認します。ここから PR と変更ファイルを確認し、作業状況をさらに監視できます。

## cloud agent に指示を追加する

実行中のタスクを確認したところで、ハイコントラストモードの作業中にライトモードも追加するよう Copilot に依頼します。

1. ハイコントラストモードを追加しているセッションを選択します。タイトルは Copilot が付けた名前や作業状況によって異なります。

    ![ミッションコントロールのアクセシビリティセッション](../../../_images/ex6-accessibility-session.png)

2. セットアップが完了して作業が始まったことを示すメッセージが表示されるまで、数分間セッションを確認します。
3. **Steer active session while Copilot is working** ダイアログに次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot の作業中にセッションへ指示を追加するダイアログが表示されたエージェントページ](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを受け取り、作業計画に反映したことを確認します。

## Copilot の作業を待つ

Copilot は更新されたタスクに取り組みます。メッセージを送信した時点で進行中の手順が終わると、新しい要望を作業に取り込みます。

今回も完了まで数分かかります。その間に、ここまでに学んだことを振り返りましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent の監視とガイドに使うエージェントページを確認しました。このミッションコントロールを使うと、実行中・完了済みのタスクを追跡し、作業内容をレビューして、進行中のタスクに追加の指示を送れます。

次の内容を学びました。

- ミッションコントロールとエージェントページを使って cloud agent のタスクを監視する
- 進行中のセッションに追加の機能を依頼する

Copilot がアクセシビリティ機能を仕上げる間に、作成されたプルリクエストを[確認して改善][next-lesson]します。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタムエージェントについて][custom-agents]

---

| [← 前のレッスン: カスタムエージェント][previous-lesson] | [次のレッスン: Copilot の作業を改善する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
