---
title: "レッスン 8 - 機能 PR の作成とマージ"
description: "絞り込み機能、命令、スキルの更新、QA プロファイル、テストをまとめてレビューし、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能、命令の更新、スキルの更新、品質保証 (QA) プロファイル、テストはすべて1つのブランチに保存されています。まとめてレビューし、プルリクエストを作成しましょう。最初の星評価のプルリクエスト (PR) は手動でマージしました。今回は **Agent Merge** に作業を任せます。

> [!NOTE]
> 通常は機能、命令の更新、スキルの更新、QA エージェントを複数の PR に分割します。このワークショップでは手順を簡略化するため、絞り込み機能と品質改善の作業全体を1つの会話とブランチにまとめ、この PR に含めています。

このレッスンでは、次の操作を行います。

- Agent Merge と、マージまでの作業を自動化する方法を学びます。
- 機能の変更全体と検証の証拠を確認します。
- 絞り込み機能の PR を作成します。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の作業を通じて、Copilot で機能の計画、実装、検証を行いました。Tailspin Toys では、マージの承認権限は開発者が保持したまま、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、Copilot は PR をブロックしている要因に対処します。たとえば、継続的インテグレーション (CI) チェックの失敗を修正し、レビューコメントに対応し、必要に応じてリベースします。その後、リポジトリで許可されている場合は GitHub の自動マージを有効にします。

これまでは自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることができますが、有効にする前に変更をレビューして準備ができていることを確認してください。

## Agent Merge で PR を管理する

コードの作成が終わったら、全体をレビューし、PR を作成して、残りの手順を Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 一覧から **Default** を選択し、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドで新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot は PR の継続的インテグレーションを監視します。すべて成功すると、マージを実行します。
7. Copilot から「PR #14 was squash-merged successfully.」などのメッセージが届いたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限不足を回避しません。ブロックされた場合は報告された理由を確認し、リポジトリで許可されるときに、レビュー済みの変更を手動でマージします。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストの作成など、開発プロセスの一部を自動化しました。このレッスンでは、次のことを行いました。

- Agent Merge と、マージまでの作業を自動化する方法を学びました。
- 機能の変更全体と検証の証拠を確認しました。
- 絞り込み機能の PR を作成しました。
- レビュー後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は[GitHub Copilot CLI のスラッシュコマンド][next-lesson]を確認し、コンテキスト、モデル、共有、クラウドへの委任などを学びます。

## リソース

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
