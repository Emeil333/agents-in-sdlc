---
title: "演習 3 - カスタムエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

## カスタムエージェントとは

GitHub Copilot の[カスタムエージェント][custom-agents-concept]を使うと、開発ワークフローの特定のタスクや分野に合わせた AI アシスタントを作成できます。リポジトリの `.github/agents` フォルダーに Markdown ファイルを作成し、目的に応じた命令、ベストプラクティス、コーディングパターン、専門知識を Copilot に伝えます。チームの専門知識を再利用できるエージェントにまとめることもできます。たとえば、[WCAG][wcag] 準拠を確認するアクセシビリティエージェント、安全なコーディングを確認するセキュリティエージェント、一貫したテストパターンを適用するテストエージェントを作成できます。

カスタムエージェントは、プロジェクト内の `.github/agents` フォルダー、またはグローバルな `~/.copilot/agents` にある Markdown ファイルで定義します。各ファイルには、少なくとも `name` と `description` を含む YAML フロントマターと、エージェントの動作、専門知識、命令を定義する Markdown プロンプトを記述します。

### カスタムエージェントとエージェントスキルの違い

カスタムエージェントと[エージェントスキル][agent-skills-concept]には共通点があります。どちらも主に Markdown ファイルで定義され、AI が作業を行う方法を指示します。最も分かりやすい違いは、**カスタムエージェントが作業者で、スキルがツール**であることです。

カスタムエージェントは独自のコンテキストウィンドウを持ち、スキルやほかのエージェントを組み合わせて作業を進めます。この演習では、アクセシビリティ用カスタムエージェントがガイドラインに沿ってサイトをレビューし、更新します。その作業の一部として、プルリクエストワークフローのスキルや、テストを実行・管理するスキルを呼び出すこともできます。

> [!NOTE]
> カスタムエージェントの作成に唯一の正解はありません。AI を使うほかの方法と同様に、環境やシナリオに合わせて試し、改善してください。

[custom-agents-concept]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[agent-skills-concept]: https://docs.github.com/copilot/concepts/agents/about-agent-skills
[wcag]: https://www.w3.org/WAI/standards-guidelines/wcag/

この演習では、カスタムエージェントを使って次のことを学びます。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

## シナリオ

Tailspin Toys は、視覚能力や好みにかかわらず、すべての人がクラウドファンディングプラットフォームを利用できるように取り組んでいます。最近、現在のダークテーマは文字色と背景色のコントラストが足りず、読みにくいという意見が寄せられました。そこでデザインチームは、利用者が切り替えられるハイコントラストモードの追加を依頼しました。

アクセシビリティは重要なため、カスタムエージェントを使ってこの機能をすばやく実装します。

## アクセシビリティ用カスタムエージェントを確認する

アクセシビリティ用のカスタムエージェントはすでに用意されています。内容を確認して、Copilot に与えるガイダンスを把握しましょう。

codespace に戻り、アクセシビリティ用カスタムエージェントのファイルを確認します。

1. `.github/agents/accessibility.md` を開きます。
2. `name` と `description` フィールドを含む YAML フロントマターを確認します。

> [!CAUTION]
> `name` と `description` を含むフロントマターは、カスタムエージェントに必須です。

3. 続くセクションを読み、次の内容を確認します。
   - アクセシブルな Web サイトのコード生成に関する主な責務
   - アクセシビリティのベストプラクティス
   - HTML、CSS、JavaScript のコード例
   - よくある問題や間違い

## Issue を作成して割り当てる

ミッションコントロールは、環境内のエージェントを管理する場所です。Copilot cloud agent にタスクを割り当て、進行状況を監視し、追加の指示を送れます。まず、ハイコントラストモードの作成を Copilot に依頼します。

1. リポジトリを開きます。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい Issue のダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Add high contrast mode to website` と入力します。
6. **Description** に次のテキストを入力します。

    ```plaintext
    We need a high contrast mode for the site. There should be a toggle for high contrast which the user can set. It should store the setting in local storage on the browser.
    ```

7. **Create** を選択して Issue を作成します。
8. 右側で **Assign to Copilot** を選択し、割り当てのダイアログを開きます。
9. カスタムエージェントの一覧から **Accessibility agent** を選択します。

    ![カスタムエージェントと Accessibility が選択された cloud agent 割り当て画面](../../../_images/ex5-select-custom-agent.png)

10. **Assign** を選択します。
11. Copilot がバックグラウンドで作業を開始します。

## まとめと次のステップ

このレッスンでは、GitHub Copilot の[カスタムエージェント][custom-agents]を確認しました。カスタムエージェントは、特定のタスクや分野に特化した AI アシスタントです。チームの専門知識や標準を再利用できるエージェントにまとめることで、Copilot により適切な作業方法を指示できます。

次の内容を学びました。

- カスタムエージェントの定義方法
- タスクをカスタムエージェントに割り当てる方法

Copilot がハイコントラストモードを実装している間に、ミッションコントロールから[エージェントセッションを監視して指示を追加][next-lesson]します。

## リソース

- [カスタムエージェントについて][custom-agents]
- [組織でカスタムエージェントを使うための準備][org-custom-agents]
- [エンタープライズでカスタムエージェントを使うための準備][enterprise-custom-agents]

---

| [← 前のレッスン: GitHub Copilot cloud agent][previous-lesson] | [次のレッスン: エージェントの監視と管理 →][next-lesson] |
|:--|--:|

[previous-lesson]: ../2-cloud-agent/
[next-lesson]: ../4-managing-agents/
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
[org-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-organization/prepare-for-custom-agents
[enterprise-custom-agents]: https://docs.github.com/copilot/how-tos/administer-copilot/manage-for-enterprise/manage-agents/prepare-for-custom-agents
