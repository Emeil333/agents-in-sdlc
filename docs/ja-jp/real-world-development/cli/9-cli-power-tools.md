---
title: "レッスン 9 - GitHub Copilot CLI のスラッシュコマンド"
description: "コンテキストの管理、モデルの選択、セッションの共有、任意のクラウド委任に使うスラッシュコマンドを確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

多くの CLI ツールと同様に、GitHub Copilot CLI にもさまざまなスラッシュコマンドがあります。これらのコマンドを使って、高度な機能、内部情報、追加の構成オプションを利用できます。これまで `/diff`、`/mcp`、`/skills`、`/agent`、`/pr` などを使いました。次は、ほかの便利なコマンドを確認します。

このレッスンでは、次の操作を行います。

- `/context` と `/compact` を使って Copilot の会話コンテキストの管理方法を確認します。
- `/model` を使って利用可能なモデルを確認します。
- `/share` でセッションをエクスポートまたは共有する方法を学びます。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認します。

## シナリオ

CLI の主なワークフローを完了しました。次に、コンテキストの管理、モデルの切り替え、セッションの共有、[Copilot cloud agent][about-cloud-agent]への任意の委任など、ほかの機能を確認します。

## Copilot CLI のコンテキストを確認する

大規模または複雑なタスクでは、モデルの最大コンテキストウィンドウに達することがあります。Copilot CLI は必要に応じて会話を自動的に圧縮します。スラッシュコマンドを使って、自分で確認したり圧縮したりすることもできます。

1. Codespace に戻り、Copilot CLI が起動していない場合は、リポジトリのルートから起動します。
2. 次のコマンドを入力します。

   ```plaintext
   /context
   ```

3. モデル、現在のトークン使用量、システム命令、ツール、メッセージ、空き領域にコンテキストがどのように分けられているかを確認します。
4. 会話を圧縮します。

   ```plaintext
   /compact
   ```

5. もう一度 `/context` を入力して、結果を比較します。会話が短い場合は、大きな違いが見られないことがあります。

> [!NOTE]
> コンテキストウィンドウが埋まると、Copilot CLI は自動的にコンテキストを圧縮します。自分でタイミングを選ぶ場合は `/compact` を使います。無関係な新しいタスクに取り組むため会話をリセットするには、`/clear` または `/new` を使います。

## モデルを選ぶ

モデルごとに得意分野が異なり、開発者によって好みも異なります。Copilot CLI では、利用可能なモデルを一覧表示して使用するモデルを選択できます。

1. 次のコマンドを入力します。

   ```plaintext
   /model
   ```

2. 利用可能なモデルと使用状況に関する情報を確認します。
3. 現在のモデルを維持するか、別のモデルを選択するか、<kbd>Esc</kbd> を押して一覧を閉じます。

## セッションを共有する

チームで学んだことを共有すると、AI ツールをより効果的に使えるようになります。`/share` コマンドを使うと、セッションを Markdown ファイルまたは HTML ファイルにエクスポートしたり、共有リンクを作成したり、GitHub gist を公開したりできます。

1. `/help` を入力し、インストール済みのバージョンに表示される `/share` のオプションを確認します。
2. セッションを共有する場合は、目的に合う方法を選びます。たとえば `/share file` はローカルの Markdown ファイルにエクスポートします。
3. エクスポートした内容を誰かに送ったり公開したりする前に、内容を確認します。プロンプト、応答、プロジェクトの詳細が含まれる場合があります。

リンクや gist の公開は任意です。チームが共有を予定していないリポジトリや会話の内容を公開しないでください。

## 任意: 作業の並列化と委任

ワークショップの主要な内容は完了しました。Copilot CLI には、大きなタスクに使えるコマンドもあります。

- `/fleet` は、独立したサブタスクをサブエージェントに分割して並列実行できます。
- `/worktree` は、別のタスク用に分離された Git worktree を作成できます。
- `/delegate` は、Copilot cloud agent にタスクを送信して非同期で実行し、プルリクエストを作成する場合もあります。

これらのコマンドは worktree やリモートの作業を作成する場合があるため、任意の機能です。試すときは、作業範囲の明確な新しいタスクから始め、通常のワークフローで結果をレビューしてください。非同期のエージェント作業について詳しく学ぶ場合は、[cloud agent ワークショップ][cloud-workshop]に進んでください。

## まとめと次のステップ

Copilot CLI のスラッシュコマンドを使うと、設定を変更したり、セッションを共有したり、内部の動作を確認したりできます。このレッスンでは、次のことを行いました。

- `/context` と `/compact` を使って Copilot の会話コンテキストの管理方法を確認しました。
- `/model` を使って利用可能なモデルを確認しました。
- `/share` でセッションをエクスポートまたは共有する方法を学びました。
- 並列作業、worktree、cloud agent への委任に使う任意のコマンドを確認しました。

Copilot CLI には、ほかにも多くのスラッシュコマンドがあります。最後に、学んだ内容を[振り返り][next-lesson]、さらに学ぶための次のステップを確認します。

## リソース

- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI のコンテキスト管理][context-management]
- [Copilot cloud agent について][about-cloud-agent]

[previous-lesson]: ../8-create-pull-request/
[next-lesson]: ../10-review/
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[context-management]: https://docs.github.com/copilot/concepts/agents/copilot-cli/context-management
[about-cloud-agent]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-workshop]: ../../cloud/
