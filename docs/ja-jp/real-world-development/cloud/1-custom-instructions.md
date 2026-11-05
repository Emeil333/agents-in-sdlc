---
title: "演習 1 - カスタム命令 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: GitHub Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。タスクの進め方に決まりがある場合や、Copilot が知っておくべき背景情報がある場合は、そのコンテキストを参照できるようにします。[命令ファイル][instruction-files]を使うと、Copilot に「何を」するかだけでなく、「どのように」行うかも伝えられます。

この演習では、次の操作を行います。

- リポジトリのカスタム命令とパスで対象を限定した命令ファイルを使い、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、ここでは変更前後のプロンプトを比較しません。Copilot cloud agent は GitHub Issue に対して非同期に動作するため、リアルタイムで並べて効果を示すのが難しいためです。後の演習で cloud agent が作成したプルリクエストをレビューするときに、命令ファイルの影響を確認できます。

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

## リポジトリ全体に適用する標準を追加する

次に、コード内のドキュメントに TSDoc コメントとファイル単位のコメントヘッダーを使う規則を追加します。後の演習で Issue を割り当てると、cloud agent がこの規則に従います。

編集の前に、作業ブランチを作成します。cloud agent は Issue の対象ブランチにある命令を読み取りますが、ここでは編集内容を適切にコミットします。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

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

## 命令の変更をコミット、プッシュ、マージする

cloud agent は、Issue の対象ブランチにある命令ファイルを読み取ります。次の演習で Issue を Copilot に割り当てたときに命令を適用するには、変更を `main` に反映しておく必要があります。

1. ステージングしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` に向けてプルリクエストを作成し、マージします。次の演習で割り当てる Issue に取り組む際、cloud agent はこの命令を読み取ります。

> [!TIP]
> このワークショップで `main` に直接作業したい場合は、ブランチ作成を省略して `main` にコミットしてもかまいません。実際のプロジェクトでの作業方法を体験できるよう、ここではブランチを使っています。

## まとめと次のステップ

Copilot がこのプロジェクトの命令ファイルからコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。残りのハーネスで cloud agent が作成するプルリクエストをレビューするときに、この標準が適用されているか確認できます。

次に、[最初の Issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズに使う命令ファイル][instruction-files]
- [カスタム命令を作成するためのベストプラクティス][instructions-best-practices]
- [Copilot 向けのより良いカスタム命令を書くための5つのヒント][copilot-instructions-five-tips]
- [命令ファイル、カスタムエージェントなどのリソース集: Awesome Copilot][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
