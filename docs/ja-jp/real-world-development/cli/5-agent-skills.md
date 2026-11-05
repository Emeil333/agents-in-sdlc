---
title: "レッスン 5 - 品質チェック用スキルをカスタマイズして使う"
description: "既存の quality-checks スキルを確認し、結果の書式をカスタマイズして、フィルタリングを検証するために使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

開発では、コードを書く以外にもやるべきことがあります。これまではコードが手動で動作することを確認し、命令ファイルを使って標準に従うようにしてきました。では、テストや lint、継続的インテグレーション (CI) のほかの作業はどうでしょうか。

このような作業には、**エージェントスキル**が最適です。スキルを使うと、こうした操作を適切に実行する方法を Copilot に伝えられます。

このレッスンでは、次の操作を行います。

- 既存の`quality-checks`スキルを確認します。
- 結果の書式をカスタマイズします。
- スキルを再読み込みして実行します。

## シナリオ

Tailspin Toys では、ユニットテスト、lint、型チェックに`quality-checks`スキルを使用しています。チームは、結果を読みやすくするため、レポートを改善したいと考えています。

## 命令、スクリプト、リソース

エージェントスキルは、エージェントが必要に応じて読み込む再利用可能なタスク手順、実行可能なスクリプト、補助リソースをまとめたものです。基本的には、スキル名をフォルダー名とし、その中に`SKILL.md`という名前の Markdown ファイルを置きます。Markdown ファイルには、そのスキルの内容と呼び出すタイミングを定義する`name`と`description`を含むフロントマターと、概要やガイダンスを記述します。スキルのフォルダーには、呼び出されたときに使用するスクリプトやほかのリソース用のサブフォルダーを含めることもできます。

> [!NOTE]
> スキルに追加のフォルダーやファイルは必須ではありません。Tailspin Toys の`quality-checks`スキルは、プロジェクトに既存のコマンドを使うため、`SKILL.md`のみで構成されています。

スキルは、チームで共有して再利用できるリポジトリ資産としてプロジェクトの`.github/skills`フォルダーに配置するか、ユーザーのスキルフォルダー`~/.copilot/skills`に配置できます。

## スキルを確認する

Tailspin Toys チームがユニットテスト、lint、型チェックを実行するために作成した`quality-checks`スキルを確認しましょう。

1. Codespace に戻ります。Codespaces エディターで`.github/skills/quality-checks/SKILL.md`を開きます。
2. ファイル上部の`name`と`description`を読みます。`description`は、Copilot がスキルを呼び出すタイミングを判断するのに役立ちます。
3. 命令を読み、テストと lint の手順がどのように示されているか確認します。
4. スキルに**Results output formatting**セクションがまだないことを確認します。

## 変更を加える前にスキルを実行する

Copilot CLI からスキルを直接呼び出すか、自然な言葉で実行を依頼できます。スキルに3つのチェックを実行させましょう。

1. Interactive モードで、フィルタリングに関する会話に戻ります。
2. 次のプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後に出力されるレポートを確認します。

## レポートをカスタマイズする

何が実行され、成功したかどうか、ツールが実際に報告した内容を示す、よりよいレポートにしましょう。結果をこの形式で報告するようにスキルを更新します。

1. `.github/skills/quality-checks/SKILL.md`に戻ります。
2. ファイルの末尾に次のセクションを追加します。

   ```markdown
   ## Results output formatting

   Upon completion, report each command that ran and whether it passed, failed, or was blocked. Include test counts, durations, errors, warnings, and other metrics only when the tool reports them. Identify the next action for any failure or blocker, and never describe a skipped or incomplete check as passed.
   ```

3. ファイルは自動的に保存されます。

## 更新したスキルを実行する

変更を加えたので、スキルを実行してみましょう。Copilot CLI は会話を再起動せずに、編集されたスキルを再読み込みできます。

1. 次のコマンドを入力します。

   ```plaintext
   /skills reload
   ```

2. 先ほどとまったく同じプロンプトを使います。

   ```plaintext
   Run the quality-checks skill for unit tests, lint, and type checks.
   ```

3. 最後のレポートを確認し、最初のレポートと比較します。

## まとめと次のステップ

既存のエージェントスキルをカスタマイズして使用しました。このレッスンでは、次の操作を行いました。

- ユニットテスト、lint、型チェック用の`quality-checks`スキルを確認しました。
- 結果の書式をカスタマイズしました。
- スキルを再読み込みして実行しました。

この変更は、フィルタリング機能の PR に含めます。次は[Playwright MCP サーバーを使って][next-lesson]、Copilot がサイトを直接操作できるようにします。

## スキルの例

以下は参考となるコミュニティの事例であり、このレッスンの追加タスクではありません。

- [Agent Skills 仕様][skill-spec]
- [コントリビューションのワークフロー: `make-repo-contribution`][contribution-example]
- [要件ドキュメント: `prd`][prd-example]
- [図と同梱のエクスポートスクリプト: `drawio`][drawio-example]
- [ブラウザーテスト: `webapp-testing`][browser-example]

[next-lesson]: ../6-mcp-playwright/
[skill-spec]: https://agentskills.io/specification
[contribution-example]: https://github.com/github/awesome-copilot/tree/main/skills/make-repo-contribution
[prd-example]: https://github.com/github/awesome-copilot/tree/main/skills/prd
[drawio-example]: https://github.com/github/awesome-copilot/tree/main/skills/drawio
[browser-example]: https://github.com/github/awesome-copilot/tree/main/skills/webapp-testing
