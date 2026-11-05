---
title: "演習 1 - カスタム命令 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot とコンテキストを共有する方法はいくつかあります。その中でも重要なのが、コード生成の指針を伝える[命令ファイル][instruction-files]です。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパスで対象を限定した命令ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の命令を使って、フィルタリングに必要なデータスライスの最初の部分 (パブリッシャーヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成されるコードに新しい標準が反映されることを確認します。
- 命令の変更とフィルタリングの基礎を `main` にコミットし、次の演習で Copilot が最新のガイダンスを使えるようにします。

> [!CAUTION]
> 生成コードが設定した標準から外れる場合があります。Copilot の動作は確率的です。この演習では、命令を更新した後の傾向の変化を確認します。生成結果が文字単位ですべて一致する必要はありません。

## 命令ファイル

### シナリオ

Tailspin Toys には、開発のガイドラインと要件があります。

- データレイヤーには必ずユニットテストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードのドキュメントには TSDoc のドキュメントコメントを使用します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

命令ファイルを使って、Copilot がこうしたガイドラインに沿って作業できるようにします。

### カスタム命令

カスタム命令を使うと、コーディングスタイルや要件に関するコンテキストと設定を Copilot に伝えられます。推奨する規約やライブラリ、コードに含めたいコメントの種類などを指定できます。リポジトリ全体、または特定のファイル形式やタスクに命令を設定できます。

命令ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に渡される単一のファイルです。技術スタック、開発するものの概要、ベストプラクティスなど、チャットや CLI の多くのリクエストに関連するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル形式向けのガイドラインを記載するファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成、ユニットテストの追加などに利用できます。

> [!NOTE]
> IDE では、命令ファイルは Copilot Chat のコード生成にのみ使用され、コード補完や次の編集候補には使用されません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリレベルの命令ファイルと、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方をコード生成時に使用します。
>
> AGENTS.md や CLAUDE.md など、ほかの規格を使った命令ファイルも [Copilot でサポートされています][custom-instructions-support]。

### 命令ファイルを管理するためのベストプラクティス

このプロジェクトの例では、代表的な方法を紹介しています。

- `copilot-instructions.md` には、開発するもの、プロジェクト構成、全体に適用するコーディング標準など、プロジェクトレベルのガイダンスを記載します。
- `*.instructions.md` には、ユニットテスト、Astro コンポーネント、データレイヤーなどのファイル形式や、特定のタスクに関する命令を記載します。
- 自然な言葉を使い、明確なガイダンスと、適切なコード例・不適切なコード例を示します。

命令ファイルの作成方法は1つではありません。実際に試しながら、プロジェクトに最適な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した命令ファイル一式を用意することをお勧めします。このプロジェクトには、[UI の変更][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用の命令ファイルがあります。
>
> Copilot に命令ファイルの生成を依頼することもできます。VS Code の **Configure Chat → Generate Agent Instructions** や Copilot CLI の `/init` など、利用する環境によって方法が異なります。手順は各環境のレッスンで説明します。
>
> テンプレートやひな形を探している場合は、命令ファイル、カスタムエージェントなどのリソースをまとめた [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム命令ファイルを確認する

このリポジトリには、基本となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で確認してください。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** 内の **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのあらゆるやり取りに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの命令があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、命令を適用するファイルをリポジトリルートからの glob で指定します。ここでは、`**/*.test.ts` に一致する TypeScript テストファイルが対象です。
5. このプロジェクトのユニットテスト作成に関する命令を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` の末尾までスクロールします。`unit-tests.instructions.md` などの命令ファイルや、プロジェクト内の既存ファイルへのリンクがあります。大きな命令セットを再利用できる小さなファイルに分け、Copilot がコード生成時に参考にできる例を示せます。ここに記載されたパスはリポジトリルートではなく、命令ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する規則はまだありません。次の手順で追加します。

## 変更用ブランチを作成する

ここではベストプラクティスに従い、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 命令を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

カスタム命令の効果を確認するため、まず現在の命令ファイルを使ってプロンプトを送信します。後で命令を更新し、同じプロンプトを再送信して違いを確認します。

1. 前の演習で開いたエディタータブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーを配置する場所を Copilot に示します。
3. Chat ビューのエージェント一覧で **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot はプロジェクトを調べてコードを変更します。ヘルパーファイルとテストなど、複数のファイルが変更されることがあります。
6. 提案されるヘルパーは、最初の引数に `db` クライアントを受け取り、型付きのパブリッシャー配列を返す関数です。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約に基づいています。
7. 生成されたコードには TSDoc ドキュメントコメントとファイル単位のコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot の動作は確率的なため、指示がなくてもドキュメントコメントを追加する場合があります。その場合も問題ありません。命令を更新した後に動作がより一貫することが重要です。

## リポジトリ全体に適用する標準を追加する

`.github/copilot-instructions.md` は、Copilot にプロジェクトレベルの情報を提供するファイルです。コード提案の品質を高めるため、プロジェクトのコーディング標準を記載しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. **Code formatting requirements** セクションを探します。現在のコーディング標準が記載されていますが、コード内のドキュメントに関する規則がないため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直下に、次の Markdown を追加します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ全体に適用する `.github/copilot-instructions.md` と、言語、ファイル形式、タスクに対象を絞った `*.instructions.md` の両方を作成できます。今回追加したドキュメントコメントの規則のようなプロジェクト全体の標準には、リポジトリレベルのファイルが適しています。

## プロンプトを再実行して変化を確認する

1. Copilot Chat に戻り、**New Chat** を選択して会話を新しくします。
2. `src/lib/publishers.ts` を選択し、Copilot が適切なファイルに注目するようにします。
3. 先ほどと**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 今度は、提案されたファイルの先頭に次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 関数に次のような TSDoc コメントが追加されていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

これで、Copilot が新しいプロジェクト標準に従うようになりました。次の演習では、実際のコードに基づいて作業を進めます。

## 命令とブランチをコミットしてプッシュする

命令ファイルもリポジトリ内のほかのファイルと同じように、ソース管理で管理します。変更をコミットして、リポジトリにブランチをプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで `git status` を実行し、命令とヘルパーの変更が含まれていることを確認します。

   ```bash
   git status
   ```

3. 命令の変更とヘルパーの基礎に関して Copilot が変更したファイルをステージして、コミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status で追加の関連ファイル (例: テスト) が表示される場合は、それらも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する Issue と関連付けてプルリクエストを作成します。GitHub ツールはすでにプロジェクトに接続されているため、Copilot に Issue の検索、Issue をクローズする PR の作成、マージを依頼できます。

> [!NOTE]
> Copilot が GitHub 上で操作するのはこの演習が初めてです。プロジェクトテンプレートでは GitHub ツールがワークスペースにあらかじめ設定されています。Copilot が初めてツールを使うときに、VS Code で GitHub へのサインインを求められることがあります。画面の指示に従って認証してください。この接続の仕組み (Model Context Protocol) は、後の演習で学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント一覧で **Agent** を選択します。
3. 命令ファイルの更新に関する Issue を探し、現在のブランチから PR を作成するよう Copilot に依頼します。命令の更新と、新しいパブリッシャーヘルパーの基礎の両方を説明に含めます。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が Issue を検索し、PR を作成します。
5. Copilot が GitHub 操作の **Allow** を求めたら、コマンドを確認して必要に応じて許可します。
6. PR が作成されたら、次のプロンプトでマージを依頼し、ローカルのブランチを `main` に戻します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェルコマンドの **Allow** を求めたら、コマンドを確認して必要に応じて許可します。

これで GitHub Copilot の支援を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトの命令ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat で次の操作を行いました。

- コード生成プロンプトを送信し、**既存の**命令がある状態で生成されるコードを確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、生成コードに新しい標準が反映されることを確認しました。
- 命令の変更とヘルパーの基礎を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]で、Copilot がコードベース全体に機能を追加するときに命令を活用します。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム命令を書くための5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム命令][personal-instructions]
- [命令ファイル、カスタムエージェントなどのリソース集: Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
