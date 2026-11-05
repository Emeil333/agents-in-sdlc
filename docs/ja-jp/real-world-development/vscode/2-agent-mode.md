---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションの簡単な更新でも、複数のファイルの変更やテストの実行などが必要になることがあります。開発者は必要なファイルを探して変更し、テストを実行し、問題を修正します。変更漏れが見つかれば、再びファイルを修正してテストします。

こうした作業に役立つのが Copilot のエージェントモードです。

Copilot のエージェントモードは、IDE 内で自律的に作業します。開発者と同様に、プロジェクト構成を調べ、必要なファイルを変更し、テストなどのタスクを実行して、見つかった問題を自動的に修正します。エージェントモードを使って、Web サイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードと cloud agent は異なる用途に対応しています。エージェントモードは IDE 内で動作し、すばやいフィードバックとやり取りが可能です。cloud agent はチームメンバーのように非同期で作業し、Issue やプルリクエストを通じてやり取りします。

この演習では、次の操作を行います。

- Copilot エージェントモードでプロジェクトを調べ、関連ファイルを見つけて、複数の変更を連携して行います。
- GitHub Copilot エージェントモードで UI とデータレイヤーに新機能を実装します。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストをレビューします。

## シナリオ

ゲームの一覧が増えてきたため、カテゴリとパブリッシャーで絞り込めるようにします。前の演習で追加したパブリッシャーヘルパーを利用し、Copilot エージェントモードでデータレイヤー、UI、テストの残りの作業を完了します。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。Web サイトは単一の Astro アプリで、ビルド時にページを静的 HTML として生成します。ページはフロントマターから Drizzle ORM を使ってローカルの SQLite データベースを直接参照します。別のバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、必要なインタラクティブ機能は標準 DOM API を使う小規模で範囲を限定した Astro `<script>` で実装します。

### Web サイトを起動する

単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習の続きで `main` にいることを確認し、codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して Codespace 内で新しいターミナルを開きます。
3. 絞り込み機能用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーが起動すると、次のような URL が表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルに表示された localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーを実行している codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認しましょう。主なページは次のとおりです。

- **ホームページ**: タイトル、画像、説明を含むボードゲームの一覧を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を始めてください。モードとモデルの選択は演習によって異なります。必要な場合は各手順で説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加します。まずバックログを確認しましょう。テンプレートからプロジェクトを設定したときに、Issue のバックログが自動作成されています。GitHub Copilot に内容を表示するよう依頼します。

1. Chat ビューのエージェント一覧で **Agent** を選択します。エージェントモードでは、自律的に変更を計画して複数ファイルを実装し、ターミナルコマンドやツールを実行できます。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

2. 使用可能なモデル一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。手順の再現性を高めるため、作成時に使った Claude Sonnet 4.5 を記載しています。LLM の動作は確率的なため、提案内容が手順と異なる場合があります。これは通常の動作です。

> [!NOTE]
> LLM の動作は確率的なため、Copilot が異なる MCP コマンドを使う場合がありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、バックログの Issue を表示します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての Issue を一覧表示するコマンドを実行します。
5. 生成された Issue の一覧を確認します。

Copilot は利用者にとって有用だと考える順に、Issue を優先順位付けしています。

## 命令ファイルを確認する

コード生成をエージェントに依頼する前に、Copilot にコンテキストを与える命令ファイルを確認しましょう。Web サイトへの機能追加方法に関する情報を含む [UI (ユーザーインターフェイス) 命令ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` を開きます。
2. 機能追加に関するガイダンスを確認します。次の内容が含まれます。
   - アーキテクチャの概要
   - コンポーネントの設計、テスト容易性、アクセシビリティに関する原則
   - ファイル形式ごとの命令ファイルへのリンク:
     - Astro
     - Tailwind CSS
     - Drizzle

> [!TIP]
> 命令ファイルから、ほかの命令ファイルやプロジェクト内のファイルを参照できます。パスは命令ファイルを基準とした相対パスです。命令を再利用したり、複雑な内容を小さく分けたり、Copilot が参考にできる例やテンプレートを示したりできます。

## フィルタリング機能を実装する

フィルタリングには、少なくとも次の3種類の変更が必要です。

- データレイヤー (`src/lib/`) に残りの絞り込みロジックを追加または調整します。
- 絞り込み動作のテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

さらに、コードベースにマージする前にテストが成功することを確認します。Copilot エージェントモードに作業を依頼しましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、UI 命令ファイルを追加します。

   ![UI 命令ファイルを選択する画面](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント一覧で **Agent** が選択されていることを確認します。

   ![Chat ビューでエージェントを選択する画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが **Claude Sonnet 4.5** のままであることを確認します。
5. 関連するバックログ Issue の要件に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関係するファイルを探す様子を確認します。データレイヤーのヘルパー、UI、テストを見つけ、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトのファイルを調べる画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、複雑さやコードベースの規模によって数分かかることがあります。作業中、コードエディターに **Keep** と **Undo** が表示される場合があります。作業完了後はすべての変更に対して操作を選べるため、作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の許可を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。Copilot はコード生成とテストを繰り返し、エラーがないと判断するまで修正します。

   ![Copilot エージェントモードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを表示しているブラウザーに戻り、新しい機能を確認します。
11. 動作とコードを確認したら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys に機能を追加しました。次のことを学びました。

- GitHub Copilot エージェントモードで UI とデータレイヤーに新機能を実装する
- プロジェクトを調べ、関連ファイルを見つけて、複数の変更を連携して行う
- コードベースにマージする前に、生成された変更とテストをレビューする

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### 発展演習: ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだ方法を使い、Copilot にページネーションの実装を依頼します。次の点を検討してください。

- 既存の命令ファイルを使うなど、現在のベストプラクティスに従います。
- 利用者がページサイズを選べるようにするか、値を固定するかなど、ページネーションの設計を検討します。
- 希望する実装に必要なガイダンスをプロンプトで Copilot に伝えます。
- Copilot に追加のコンテキストを与えたり修正を依頼したりすることがあります。Copilot を使うときの通常の作業です。

## リソース

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
