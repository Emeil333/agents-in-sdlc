---
title: "演習 5: GitHub Copilot の作業を改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー体験の改善と機能追加を目的に、GitHub Copilot にいくつかのタスクを依頼しました。コードへのドキュメント追加、デザインチーム向けの関連ゲーム機能の実装、ハイコントラストモードとライトモードによるアクセシビリティ機能の実装を行いました。コード変更を確認し、必要に応じて Copilot に改善を依頼しましょう。

### シナリオ

生成 AI を利用しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。チームメンバーにもレビューを依頼する前に、Copilot が作成したドキュメント、関連ゲーム機能、アクセシビリティ機能の変更を確認しましょう。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ自律的に作業するため、安全性を確保する制約があります。

- Copilot のリポジトリへのアクセスは読み取り専用で、コードを書き込めるのは作業に使うブランチ**のみ**です。
- cloud agent は GitHub Actions 内で動作し、作業用の一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- [外部リソースへのアクセスは既定で制限されており](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)、MCP サーバーも対象です。

## 生成されたドキュメントをレビューする

GitHub Copilot cloud agent が最初に作成したプルリクエスト (PR)、つまりコードへのドキュメント追加をレビューします。GitHub.com の通常の PR インターフェイスを使います。

> [!NOTE]
> PR に、GitHub Copilot がファイアウォールでブロックされたという警告が表示されることがあります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定される**動作です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して、プルリクエストの一覧を開きます。
3. **Add missing documentation** などのタイトルのプルリクエストを開きます。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストに **[WIP]** が付きます。その場合は Copilot の作業が完了するまで待ちます。数分かかることがあります。その間に休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら **Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメントコメントや、その他のドキュメントを含むコードを確認します。具体的な変更内容は異なる場合があります。

    変更を確認するときは、演習 2 で確認したカスタム命令ファイルに基づく TSDoc コメント、TypeScript の規約、コメントヘッダーに注目してください。

6. 更新内容を確認できたら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。

## GitHub Copilot に変更を依頼する

Copilot とのプルリクエスト作業は一方通行ではありません。チームメンバーと同様に、プルリクエストやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを認識し、対応する新しいセッションを開始します。結果は確率的なため、必ずしも特定のテキストを使う必要はありません。次のような変更を依頼できます。

- 各コードファイルの先頭に、ファイルの役割を説明するコメントヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構成を説明する README を作成する。

1. 上記の例のいずれか、または追加したいドキュメントについて、通常のユーザーと同じように **@copilot** をメンションしてコメントを追加します。
2. **View Session** を選択して Copilot の作業を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択するとプルリクエストに戻れます。

    ![プルリクエストに戻る画面](../../../_images/ex7-back-to-pr.png)

4. Copilot の作業が完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

納得できるまで改善を繰り返してください。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![レビュー可能な状態に PR を変更する画面](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する Issue に対し、Copilot が作成した PR を確認します。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** などのタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 更新内容を確認できたら **Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す通知を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
9. **任意:** Codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えてください。例: **copilot/fix-8**)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を実装しました。前と同じように、cloud agent に変更を依頼して反復できます。たとえば関連ゲームの表示件数を調整したり、カスタム命令を更新する**前**に割り当てたタスクのため、コメントヘッダーや TSDoc コメントの追加を依頼したりできます。前と同じように、**Conversation** タブでコメントを追加すると、Copilot が新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能をレビューします。この PR には、演習 3 で依頼したハイコントラストモードと、演習 4 のミッションコントロールから追加したライトモードが含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** などのタイトルの PR を選択します。

> [!NOTE]
> Copilot が作業中の場合、プルリクエストに **[WIP]** が付きます。Copilot の作業が完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 特に次の点を確認します。
   - モードを切り替える UI コンポーネント
   - 利用者の設定を保存する local storage の使用
   - ハイコントラストモードとライトモードの CSS やスタイル変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モード切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認できたら **Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す通知を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する画面](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されます。問題がなければ、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかる場合があります。
10. **任意:** Codespace でこのブランチに切り替えて、アクセシビリティ機能を手動テストできます。Codespace に移動してターミナルを開き、次のコマンドを実行します (`<branch-name>` は Copilot が作成したブランチ名に置き換えてください)。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    アプリケーションを起動し、ブラウザーでハイコントラストモードとライトモードを切り替えます。設定がページを再読み込みしても保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の実装をガイドしたことを確認してください。アクセシビリティに関する懸念や改善点がある場合は、前の PR と同様にコメントで **@copilot** に更新を依頼できます。

## 任意の演習 — 作業の委任を続ける

cloud agent にバックログのタスクを任せると、その間に別の作業ができます。この習慣を身につけるために、リポジトリで Issue をいくつか作成して Copilot に割り当てましょう。次のようなタスクを試せます。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセスヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。レッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム命令ファイルを確認**し、cloud agent の出力にどのように影響するかを学びました。
- **Issue を Copilot cloud agent に割り当て**、環境を設定して、非同期に計画・実行する様子を確認しました。
- **アクセシビリティ用カスタムエージェントを作成して使用**し、ハイコントラストとライトモードの切り替え機能を追加しました。
- **Copilot agents ページをミッションコントロールとして使用**し、アクセシビリティのセッションを監視して指示を追加しました。
- **cloud agent のプルリクエストをレビューして改善**し、`@copilot` に変更を依頼してワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。引き続き学びたい場合は、ほかのハーネスで異なる環境から関連するシナリオを体験できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE 内で Copilot Chat のエージェントモードと MCP を使います。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、任意の `/delegate` を使い、ターミナルからレビュー済みの変更を2件リリースします。

自分のリポジトリでは、次のアイデアも試せます。

- リファクタリングやテストカバレッジの Issue を cloud agent に割り当てます。
- 別の分野に特化したカスタムエージェントを作成します。
- 異なる技術スタック向けに `copilot-setup-steps.yml` を設定します。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub Issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを設定する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
