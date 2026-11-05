---
title: "レッスン 4 - カスタム命令で Copilot をガイドする"
description: "リポジトリの命令を確認してドキュメント標準を追加し、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合は、そのガイダンスを Copilot が利用できるようにします。[命令ファイル][instruction-files]では、求めるコードだけでなく、その構造や作成方法も説明できます。絞り込み機能を実装したところで、Copilot が使った命令を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次の操作を行います。

- リポジトリの命令とパスで対象を限定した命令ファイルがエージェントに適用される方法を確認します。
- コーディング標準を守るために命令ファイルを更新します。
- 命令ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発のガイドラインと要件があります。

- コメントでは、コードの言い換えではなく、意図や自明でない設計判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。`db` 引数を注入する関数では、その引数についても記載します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連するコードを変更したときにコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

命令ファイルを使って、Copilot がこうしたガイドラインに沿って作業できるようにします。

## 命令ファイル

カスタム命令を使うと、コーディングスタイルや要件に関するコンテキストと設定を Copilot に伝えられます。推奨する規約やライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体、または特定のファイル形式やタスクに命令を設定できます。

命令ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一のファイルです。多くのリクエストに関連するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル形式、タスクに関するガイドラインを記載するファイルです。

> [!NOTE]
> ほかの命令ファイル形式への対応は、環境によって異なります。特定の形式を使用する前に、[カスタム命令のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム命令ファイルを確認する

スターター プロジェクトには、いくつかの命令ファイルが用意されています。変更を加える前に確認し、その影響を調べましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディングガイダンスを確認します。この命令は、リポジトリ内で Copilot と行うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの命令があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドでは、命令を適用するファイルを glob で指定します。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの命令ファイルや既存のプロジェクトファイルへのリンクを確認します。大きな命令セットを再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームのガイダンスに合わせて命令ファイルを更新する

既存のファイルは良い出発点ですが、追加すべき内容があります。中心となる `copilot-instructions.md` を変更し、新しく生成される TypeScript に TSDoc コメントが追加されるようにします。

1. `.github/copilot-instructions.md` で **Code formatting guidance** というセクションを探します。35行目前後にあります。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを使う

Copilot CLI は会話の開始時にリポジトリの命令を読み込みます。機能のコンテキストを保ったまま新しいガイダンスを読み込むには、命令を保存した後に絞り込みの会話を終了して再開します。

1. 絞り込みの会話が開いている場合は `/exit` を入力して閉じます。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチのリポジトリルートで、保存した会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示された場合は絞り込みの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. 次のプロンプトを送信し、更新したガイダンスを適用するよう Copilot に依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が命令ファイルからコンテキストを取得する方法を確認し、新しい標準を機能に適用しました。このレッスンでは、次のことを行いました。

- リポジトリの命令とパスで対象を限定した命令ファイルがエージェントに適用される方法を確認しました。
- コーディング標準を守るために命令ファイルを更新しました。
- 命令ファイルがコードに与える影響を確認しました。

次は、再利用可能な `quality-checks` スキルを[カスタマイズして実行][next-lesson]し、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI のカスタム命令を追加する][instruction-files]
- [カスタム命令のサポート][custom-instructions-support]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [命令ファイル、カスタムエージェントなどのリソース集: Awesome Copilot][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
