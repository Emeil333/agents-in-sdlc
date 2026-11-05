---
title: "レッスン 10 - 振り返りと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用できるカスタマイズ、追加のリソースを振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使い、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備して Copilot CLI をインストールし、プロジェクトを確認して、登録済みの絞り込み Issue を見つけました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込み Issue から始めて、Plan モードで方針を決め、Autopilot モードで実装し、Interactive モードでレビューしました。
- カスタム命令でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニットテスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 品質保証 (QA) カスタムエージェントを作成して選択し、要件、カバレッジ、スキルの結果、ブラウザーの証拠を評価しました。
- 絞り込み機能全体をレビューし、絞り込み PR に Agent Merge を使うことを承認しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## リリースしたもの

ワークショップには、更新された `main` からそれぞれ独立したブランチを作成する2つの PR があります。

1. **星評価:** ゲームカードに既存の `starRating` と未評価の状態を明示して表示します。
2. **絞り込みと品質のワークフロー:** 絞り込み機能を実装し、命令を更新して機能に適用し、`quality-checks` の報告形式をカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

絞り込みの計画から PR の作成までは、同じ会話とブランチで作業しました。ワークショップを簡略化するため、これらの作業を1つの PR にまとめています。

## さまざまな方法での検証

自動テスト、自分で行うブラウザー確認、MCP を通じた Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルはユニットテスト、lint、型チェックを実行し、新しい形式で報告しました。QA はこれらの結果と、要件やテストカバレッジの確認をまとめました。

追加するテストは、実際に不足しているカバレッジを補うものである必要があります。QA の実行時にテスト追加が不要と判断されても、問題ありません。マージを許可する前に、コードと検証結果をレビューし、変更後は影響する検証を再度行います。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業結果を左右します。このワークショップでは命令を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを設定しました。これらのカスタマイズを会話間で再利用し、チームのニーズに合わせて調整できます。命令は標準を設定し、スキルは繰り返し実行するタスクを説明し、カスタムエージェントは専門家の役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合った**モードとモデル**を選びます。**Plan** は実装前の方針作りに、**Interactive** は範囲を絞った変更を確認しながら進める場合に、**Autopilot** は明確に定義したタスクを任せる場合に使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

インフラと同じように、コンテキストも重要です。何を、なぜ、どのように作るかを明確に説明すると、結果が大きく変わります。

## さらに試す

主要なワークフローを確認しました。CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を話し合い、別の視点を得ます。
- `/fleet` で独立したサブタスクを並列に調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

どのツールも、上達するには使い続けることが大切です。本番コード、個人のコード、何年も作りたいと思っていた小さなアプリなどで活用してみてください。学んだことをチームと共有し、お互いの経験から学びましょう。ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張したい場合は、省略可能な [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境での作業を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、または [Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
