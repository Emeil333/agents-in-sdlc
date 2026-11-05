---
title: "レッスン 2 - 星評価の追加: 小さな成果"
description: "Copilot CLI でゲームカードに小さな変更を加え、転送したブラウザーで確認して、最初のプルリクエストとしてマージします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

Copilot CLI をインストールし、会話を試しました。次はプロジェクトに最初の変更を加えます。小さな変更から始めましょう。ゲームのデータには星評価がありますが、ホームページのゲームカードにはまだ表示されていません。エージェントに表示を依頼し、変更を確認して、最初のプルリクエストとしてマージします。

このレッスンでは、次の操作を行います。

- 機能ブランチで Copilot との会話を開始します。
- エージェントにプロジェクトの小さな変更を依頼します。
- `/diff` で変更をレビューします。
- アプリを起動し、転送したブラウザーで変更を確認します。
- 最初のプルリクエストを作成してマージします。

## シナリオ

Tailspin Toys の各ゲームには星評価を設定できます。ゲーム詳細ページにはすでに評価が表示されますが、ホームページのゲームカードにはタイトル、カテゴリ、パブリッシャー、説明しか表示されていません。ウォームアップとして、各カードに既存の評価を表示させます。これは最初のセッションに適した、小規模で独立した変更です。

## 会話の構成

**会話**では、タスクについて Copilot CLI とやり取りします。Copilot app とは異なり、通常の CLI 会話ではターミナルで現在チェックアウトしているリポジトリと Git ブランチを使用し、専用の worktree は作成しません。保存した会話を後で再開できます。ファイルとブランチは通常の Git 状態としてディスク上に残ります。

会話では、プロンプトとエージェントの応答、ファイルを調査・編集する際のツール操作、`/diff` で確認できる変更の3つを確認できます。

## 会話を開始して変更を依頼する

新しい会話を開始し、機能を実装します。

1. Codespace に戻ります。
2. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押します。
3. まだ起動していない場合は、次のコマンドを実行します。

   ```bash
   copilot --yolo
   ```

4. スラッシュコマンド `/new` を入力して <kbd>Enter</kbd> を押し、新しいセッションを開始します。
5. 次のプロンプトを送信して変更を依頼します。

   ```plaintext
   Show each game's starRating out of 5 in the game cards on the list page. If the rating is null, show "No rating yet". Keep the card layout as it is, add tests, and run the relevant checks.
   ```

Copilot はプロジェクトを調べ、ゲームの詳細を表示するファイルを見つけ、必要なコードを作成します。Copilot CLI で新機能を追加できました。

## 差分をレビューする

小さな変更でも、マージする前に AI が生成した変更をレビューしてください。Copilot CLI 内で変更を確認しましょう。

1. `/diff` を入力し、変更されたすべてのファイルを確認します。
2. ゲームカードに評価がある場合は数値が、`starRating` が `null` の場合は `No rating yet` が表示されることを確認します。
3. どちらの状態もテストで確認されていることを確認します。
4. Copilot が実行したチェックの結果を確認し、失敗があれば修正を依頼します。
5. レビューが終わったら <kbd>Esc</kbd> を押して差分画面を終了します。

> [!NOTE]
> すべての生成 AI ツールと同様、Copilot の動作は確率的です。生成コードは異なる場合があります。特定の実装に固執せず、動作をレビューしてください。

## 変更を確認する

コードを読んだだけで正常に動くと判断せず、Copilot に Web サイトを起動させ、Codespaces で転送したブラウザーから更新された UI を確認します。

1. Copilot に次のプロンプトを送信して、アプリの起動を依頼します。

   ```plaintext
   Start the app so I can inspect the star-rating change in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能になったら **Open in Browser** を選択します。
3. ゲームカードに5段階の評価が表示されていることを確認します。
4. Copilot に戻り、次のプロンプトで開発サーバーを停止します。

   ```plaintext
   Stop the development server you started.
   ```

## 最初のプルリクエストを作成してマージする

機能を作成しました。新しいコードをプロジェクトにマージするため、プルリクエスト (PR) を作成します。

1. 既定のエージェントに次のプロンプトを送信し、変更のコミットを依頼します。

   ```plaintext
   Commit the reviewed star-rating changes with an appropriate commit message.
   ```

2. `/pr create` を入力します。Copilot CLI は PR 作成時に既存のコミットをプッシュし、PR の URL を表示します。
3. Copilot CLI が表示した URL を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して PR を開きます。
4. 変更されたファイルとチェックの結果を確認します。
5. 準備ができたら **Merge pull request** を選択し、マージを確定します。
6. Codespace に戻り、`/exit` で Copilot CLI を終了します。
7. ローカルの `main` を更新します。

   ```bash
   git checkout main
   git pull
   ```

## まとめと次のステップ

GitHub Copilot CLI を使って、最初の変更をリリースしました。具体的には、次の作業を行いました。

- 機能ブランチで Copilot との会話を開始しました。
- エージェントにゲームカードの小さな変更を依頼しました。
- `/diff` で変更をレビューしました。
- アプリを実行し、転送したブラウザーで星評価を確認しました。
- 最初のプルリクエストを作成してマージしました。

次は[絞り込み Issue から始め、Plan と Autopilot の各モードを使って][next-lesson]大きな機能を構築します。

## リソース

- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../1-install-copilot-cli/
[next-lesson]: ../3-agent-modes/
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
