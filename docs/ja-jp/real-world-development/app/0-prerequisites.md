---
title: "レッスン 0 - 前提条件"
description: "GitHub Copilot app のレッスンに向けて、Tailspin Toys プロジェクト用の Node.js をインストールし、テンプレートからリポジトリの自分用コピーを作成します。"
authors:
  - geektrainer
lastUpdated: 2026-10-22
---

GitHub Copilot app は、Copilot と GitHub の両方を一元的に扱うデスクトップアプリです。Issue や pull request にすばやくアクセスでき、GitHub Copilot を使った開発もできます。このワークショップでは、Astro で構築された Tailspin Toys アプリを GitHub Copilot app で更新しながら、ローカル環境で作業します。始める前に、Node.js がローカルにインストールされていることを確認し、Copilot app をインストールしましょう。

このレッスンでは、次の操作を行います。

- プロジェクトのテストを実行できるよう、Node.js をインストールします。
- テンプレートから Tailspin Toys プロジェクトの自分用コピーを作成します。

## Node.js をインストールする

いくつかのレッスンでは、エージェントに機能を実装させ、Tailspin Toys のテストスイートをローカルで実行します。そのためには **[Node.js][nodejs]** が必要です。Node.js はプロジェクトに必要な唯一のランタイムです。現在の **LTS** リリースをインストールしてください。

すべてのプラットフォームで最も簡単なのは、公式インストーラーを使う方法です。

1. Windows Terminal、macOS のターミナルなど、普段使用しているターミナルを開きます。
2. 次のコマンドを実行し、インストールされている Node.js のバージョンを確認します。

    ```shell
    node --version
    npm --version
    ```

3. どちらのコマンドも成功し、Node.js のバージョンがプロジェクトの README と `package.json` に記載された要件を満たしている場合は、次のセクションに進めます。

> [!TIP]
> Node.js がインストールされていない場合、または更新が必要な場合にのみ、以降の手順を実行してください。

4. [Node.js のダウンロードページ][node-download]を開きます。
5. 使用しているオペレーティングシステム向けの **LTS** ビルドをダウンロードします。
6. インストーラーを実行し、既定の設定を選択します。Windows では **Add to PATH** を選択したままにします。
7. インストール後、新しいターミナルを開きます。
8. 新しいターミナルで次のコマンドを実行し、インストールを確認します。

    ```bash
    node --version
    npm --version
    ```

9. インストールされた Node.js と npm のバージョンが表示されます。

> [!IMPORTANT]
> Copilot app のエージェントも `node --version` と `npm --version` を実行できる必要があります。普段使っているターミナルで成功しても、アプリから利用できるとは限りません。Node.js のインストール後や PATH の変更後は、アプリを完全に終了してから再起動してください。セッション中にいずれかのコマンドが引き続き失敗する場合は、続行する前に[セッション環境のトラブルシューティング手順][session-environment]を確認してください。

> [!IMPORTANT]
> E2E チェックを実行するには、各 worktree にプロジェクトの依存関係と Playwright Chromium も必要です。worktree の準備方法は Tailspin Toys リポジトリの README に従い、インストール要求は内容を確認してから承認してください。

## ラボ用リポジトリを設定する

Tailspin Toys プロジェクトの自分用コピーを使って作業します。[テンプレートリポジトリ][template-repository]からコピーを作成してください。新しいリポジトリにはラボに必要なファイルがすべて含まれます。次のレッスンで、このリポジトリをアプリに接続します。

1. 新しいブラウザーウィンドウで、このラボの GitHub リポジトリ `https://github.com/github-samples/tailspin-toys` を開きます。
2. ラボ用リポジトリのページで **Use this template** ボタンを選択し、続けて **Create a new repository** を選択して、自分用のコピーを作成します。

    ![Use this template ボタンのドロップダウンで Create a new repository が選択されている画面](../../../_images/app-0-use-template.png)

3. GitHub または Microsoft が主催するイベントでワークショップに参加している場合は、メンターの指示に従ってください。それ以外の場合は、GitHub Copilot を利用できる Organization に新しいリポジトリを作成できます。

    ![github-samples/tailspin-toys がテンプレートに設定され、リポジトリ名が入力された Create a new repository フォーム](../../../_images/app-0-create-repository.png)

4. 作成したリポジトリのパス (**organization-or-user-name/repository-name**) をメモしてください。ラボで後ほど使用します。

> [!NOTE]
> テンプレートからリポジトリを作成すると、GitHub Issue のバックログが自動的に作成されます。ワークショップではこれらの Issue に取り組むため、自分で Issue を作成する必要はありません。

ワークショップ用テンプレートの新しいコピーを使ってください。リポジトリの命令、アプリケーションのコード、テスト、quality-checks スキル、既存のキャンバス拡張機能が含まれています。ワークショップでは、スキルをカスタマイズし、QA エージェントを作成します。古いコピーを使用する場合は、必要なファイルが含まれていることをファシリテーターに確認してください。

## まとめと次のステップ

準備が整いました。このレッスンでは、次の操作を行いました。

- 自分のコンピューターでプロジェクトをビルドおよびテストできるよう、Node.js をインストールしました。
- テンプレートから Tailspin Toys リポジトリの自分用コピーを作成しました。

次は[GitHub Copilot app をインストールし][next-lesson]、作成したリポジトリを接続して、ワークスペースの使い方を確認します。

## リソース

- [Node.js をダウンロードする][node-download]
- [テンプレートからリポジトリを作成する][template-repository]
- [GitHub Copilot app について][about-copilot-app]

[next-lesson]: ../1-install-copilot-app/
[nodejs]: https://nodejs.org/
[node-download]: https://nodejs.org/en/download
[template-repository]: https://docs.github.com/repositories/creating-and-managing-repositories/creating-a-template-repository
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[session-environment]: ../3-agent-modes/#issue-からセッションを開始する
