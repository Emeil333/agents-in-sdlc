---
title: "レッスン 1 - GitHub Copilot CLI をインストールする"
description: "Codespace に Copilot CLI をインストールして認証し、使い方を確認して、登録済みの絞り込み Issue を見つけます。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

[GitHub Copilot CLI][about-copilot-cli] は、ターミナルで動作するエージェント型コーディングアシスタントです。コマンドラインからコードベースを調べ、コードを生成し、コマンドを実行し、外部ツールを利用できます。タスクを任せたり、変更を依頼したりしながら、作業に集中できます。まずツールをインストールしましょう。使い慣れたツールでインストールできます。

このレッスンでは、次の操作を行います。

- npm で GitHub Copilot CLI をインストールします。
- GitHub アカウントで認証します。
- ワークショップのリポジトリを信頼し、簡単な会話を試します。
- 組み込みの GitHub MCP サーバーで絞り込み Issue を見つけます。

## シナリオ

チームでは、増え続けるバックログに取り組むために AI エージェントを使い始めています。Copilot CLI を使うと、多くの開発者が日常的に作業するターミナルでその機能を利用できます。このレッスンでは、残りのワークショップで使えるように、インストールと認証を済ませます。

## Copilot CLI をインストールする

Copilot CLI は [npm][install-cli]、WinGet、Homebrew からインストールできます。GitHub Codespaces には Node.js があらかじめインストールされているため、ここでは npm を使います。

1. Codespace に戻り、ターミナルを開きます。
2. Node.js がインストールされていて、必要なバージョンを満たすことを確認します。

   ```bash
   node --version
   ```

   バージョン 24 以降が表示されます。

3. Copilot CLI をグローバルにインストールします。

   ```bash
   npm install -g @github/copilot
   ```

4. インストールを確認します。

   ```bash
   copilot --version
   ```

   バージョン番号が表示されます。

## GitHub で認証する

Copilot CLI の初回起動時に、GitHub アカウントによる認証を求められます。

1. Copilot CLI を起動します。

   ```bash
   copilot
   ```

2. 表示された場合は、デバイスコードの手順に従って認証し、Copilot CLI を承認します。
3. Copilot CLI に次のプロンプトが表示されます。

   ```plaintext
   Copilot can read files in this folder and, with your permission, edit them or run code and shell commands. It will remember your permissions for the rest of this session.

   Do you trust the files in this folder?
   ```

4. 表示されたパスが Tailspin Toys リポジトリであることを確認し、**Yes, and remember this folder for future sessions** を選択して信頼します。

> [!NOTE]
> Codespace では GitHub セッションを使ってすでに認証されている場合があります。認証のプロンプトが表示されずに Copilot CLI が起動した場合は、そのまま続行できます。

## 使い方を確認する

通常のシェルプロンプトでは Codespace 上でコマンドが実行されます。Copilot CLI を起動した後は、自然な言葉でエージェントに指示し、スラッシュコマンドで会話を操作します。

1. `/model` を入力し、矢印キーで **Auto** を選択して <kbd>Enter</kbd> を押し、もう一度 <kbd>Enter</kbd> を押して確定します。
2. `/help` を入力して、インストール済みのバージョンで利用できるコマンドを確認し、<kbd>Esc</kbd> を押してヘルプ画面を閉じます。
3. 次の質問を Copilot に送り、動作を確認します。

   ```plaintext
   What are the key files in this project?
   ```

4. 応答を読み、Copilot が回答前にリポジトリを調べることを確認します。
5. `/mcp list` を入力し、組み込みの GitHub MCP サーバーが利用可能であることを確認します。
6. 次のプロンプトで、絞り込み Issue を探すよう Copilot に依頼します。

   ```plaintext
   Using GitHub MCP, find the issue in this repository titled "Allow users to filter games by category and publisher." Give me its URL and a short summary. Don't change anything.
   ```

7. URL を開き、Issue を読みます。最初の小さな変更を終えた後、この Issue に取り組みます。

> [!TIP]
> 通常の Copilot CLI セッションは、ターミナルで現在チェックアウトしているブランチを使います。自動的に worktree が作成されることはありません。各変更の前に機能ブランチを作成します。

## ワークショップ用のショートカットを使う

Copilot CLI は通常、許可された範囲外のツールを使う前に確認を求めます。このワークショップでは `--yolo` を付けて再起動します。Codespace 内で承認プロンプトを省略できるようにする、ユーザーが承認したショートカットです。

> [!CAUTION]
> `--yolo` は、すべてのツール、パス、URL への自動アクセス (`--allow-all-tools`、`--allow-all-paths`、`--allow-all-urls`) を有効にします。Codespace や VM など、分離された環境でのみ使い、日常の開発で既定の設定にしないでください。詳しくは[ツールの使用を許可または禁止する][allow-all-warning]を参照してください。

このワークショップでは `--enable-all-github-mcp-tools` を使い、後のレッスンで Issue やプルリクエストを操作するための GitHub MCP の読み取り/書き込みツールを有効にします。Codespace によってローカルコンピューターへのアクセスは制限されていますが、認証済みの GitHub リソースは実際のものです。公開やマージの前に変更をレビューしてください。

1. `/exit` で Copilot CLI を終了します。
2. リポジトリルートから再起動します。

   ```bash
   copilot --yolo --enable-all-github-mcp-tools
   ```

3. もう一度プロジェクトについて簡単な質問をして会話が動くことを確認し、`/exit` で終了します。

Copilot は会話を自動的に保存します。後で命令を更新したりエージェントを追加したりした後に `copilot --resume` を使うと、同じ会話とブランチに戻れます。

## まとめと次のステップ

おめでとうございます。このレッスンでは、次の操作を行いました。

- npm で GitHub Copilot CLI をインストールしました。
- GitHub アカウントで認証しました。
- ワークショップのリポジトリを信頼し、簡単な会話を試しました。
- 組み込みの GitHub MCP サーバーで絞り込み Issue を見つけました。

次は[最初の小さな変更に取り組み][next-lesson]、Copilot CLI でゲームカードに星評価を表示します。

## リソース

- [GitHub Copilot CLI をインストールする][install-cli]
- [GitHub Copilot CLI について][about-copilot-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-add-star-rating/
[install-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/set-up-copilot-cli/install-copilot-cli
[about-copilot-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[allow-all-warning]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli#allowing-and-denying-tool-use
