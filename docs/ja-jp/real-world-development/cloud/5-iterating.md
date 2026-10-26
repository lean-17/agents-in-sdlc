---
title: "演習 5: GitHub Copilot の作業を反復して改善する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザー エクスペリエンスの改善と機能追加を目的に、GitHub Copilot に複数のタスクを依頼してきました。コードへのドキュメント追加、デザイン チームが改善を続けられる関連ゲーム機能の作成、ハイ コントラストとライト モードの切り替えを含むアクセシビリティ機能の実装を依頼しました。変更内容を確認し、必要であれば Copilot にフィードバックを送り、修正してもらいましょう。

### シナリオ

これまで何度も確認したように、生成 AI を導入してもソフトウェア設計と DevOps の基本は変わりません。生成されたコードは常にレビューし、通常の DevOps プロセスに従います。チームでレビューを有効にする前に、ドキュメント、関連ゲーム機能、アクセシビリティ機能について GitHub Copilot が提案した内容を確認します。

## セキュリティと GitHub Copilot cloud agent

Copilot cloud agent は非同期かつ監督なしでタスクを実行するため、安全性を保つためのセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセスのみを持ち、書き込みはコード作成に使うブランチ**のみ**に許可されます。
- Cloud agent は GitHub Actions 内で、作業用の独立した一時環境を作成します。
- GitHub Actions ワークフローは、人間が承認するまで実行されません。
- MCP サーバーを含め、[外部リソースへのアクセスは既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、コードにドキュメントを追加するために GitHub Copilot cloud agent が作成した最初のプルリクエスト (PR) を確認します。GitHub.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認すると、GitHub Copilot がファイアウォールによってブロックされているという警告が表示される場合があります。Copilot は MCP サーバーへの外部呼び出しを含め、既定で外部リソースへのアクセスが制限されているため、これは**想定内**です。必要に応じて、[Copilot cloud agent のファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択して PR の一覧を開きます。
3. **Add missing documentation** または同様のタイトルの PR を開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、PR には **[WIP]** が表示されます。その場合は完了まで待ちます。数分かかる場合があるため、休憩したり、学んだことを振り返ったりしてください。

4. PR の準備ができたら、**Files changed** タブを選択し、変更をレビューします。

    ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメント コメントなど、更新されたコードを確認します。変更内容は状況によって異なります。

    変更を確認するときは、TSDoc のドキュメント コメント、TypeScript 規約、コメント ヘッダーを探します。これらは演習 2 の最初に確認したカスタム指示ファイルに基づいています。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの実行承認待ちを示すインジケーターが表示されていることを確認します。
8. 承認待ちのワークフローがあれば、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは一方通行ではありません。チーム メンバーと同じように、PR のコメントやコードへのインライン コメントで Copilot をタグ付けできます。Copilot はコメントを確認し、対応するための新しいセッションを開始します。結果は確定的ではないため、依頼する内容を一律には示せません。たとえば、次のような更新を依頼できます。

- 各コード ファイルの先頭に、そのファイルの役割を簡潔に説明するコメント ヘッダーを追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメント コメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記のアイデアなどを参考に、コードベースに追加したいドキュメントの変更を依頼するコメントを追加します。ほかのユーザーと同じように **@copilot** をタグ付けします。
2. **View Session** を選択して、Copilot が作業する様子を確認します。Copilot が更新用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、PR に戻れます。

    ![Back to pull request](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、PR に新しいコミットが表示されます。
5. **Files changed** タブを選択して変更をレビューします。

問題がなくなるまで反復できます。完了したら、PR をドラフトからレビュー準備完了に変更して、`main` ブランチにマージします。

![PR をレビュー準備完了に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue に対して、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** または同様のタイトルの PR を選択します。
4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 変更を確認して問題がなければ、**Conversation** タブに戻って下にスクロールします。
6. ワークフローの実行承認待ちを示すインジケーターが表示されていることを確認します。
7. 承認待ちのワークフローがあれば、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

8. PR のチェック セクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
9. **オプション:** Codespace でブランチに切り替えて、関連ゲーム機能を手動でテストできます。Codespace に移動し、ターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

Copilot が関連ゲーム機能を作成しました。これまでと同じように、Copilot cloud agent に更新を依頼して反復できます。たとえば、関連ゲームの表示数を変更したり、コメント ヘッダーや TSDoc コメントを追加したりできます (この issue はカスタム指示を更新する**前**に割り当てたことを思い出してください)。前と同様に、**Conversation** タブでコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ カスタム エージェントで実装した機能を確認します。この PR には、演習 3 で依頼したハイ コントラスト モードと、演習 4 のミッション コントロールで依頼したライト モードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** または同様のタイトルの PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、PR には **[WIP]** が表示されます。その場合は完了まで待ちます。数分かかる場合があります。

4. **Files changed** タブを選択して、生成されたコードをレビューします。
5. 次の点に特に注意して実装を確認します。
   - モードを切り替える UI コンポーネント
   - ユーザーの設定を保存する local storage の使用
   - ハイ コントラスト モードとライト モードの CSS またはスタイル変更
   - アクセシビリティ属性 (ARIA ラベル、キーボード操作など)
   - モードの切り替えを管理する JavaScript/TypeScript コード

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの実行承認待ちを示すインジケーターが表示されていることを確認します。
8. 承認待ちのワークフローがあれば、**Approve and run workflows** を選択します。

   ![Approve and run workflows](../../../_images/shared-approve-workflows.png)

9. PR のチェック セクションでワークフローがキューに追加されたことを確認します。問題がなければ、単一の Astro アプリに対するプロジェクトのチェックが成功します。完了まで数分かかることがあります。
10. **オプション:** Codespace でブランチに切り替えて、アクセシビリティ機能を手動でテストできます。ターミナルを開き、Copilot が作成したブランチ名に置き換えて次のコマンドを実行します。

    ```bash
    git fetch origin
    git checkout <branch-name>
    ```

    次にアプリを起動し、ブラウザーでハイ コントラストとライト モードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ カスタム エージェントは、アクセシビリティのベスト プラクティスに沿って機能を実装するよう Copilot をガイドしました。アクセシビリティ上の懸念や改善点があれば、前の PR と同じようにコメントで **@copilot** をタグ付けして更新を依頼できます。

## オプションの演習 — 委任を続ける

Cloud agent は、バックログの実際の作業を任せて、その間ほかの作業に集中できるときに最も役立ちます。習慣として身につけるため、リポジトリに issue を追加して Copilot に割り当ててみましょう。たとえば、次のようなアイデアがあります。

- ゲーム詳細ページに支援者向け関心フォームを作成する。
- ゲーム一覧ページにページネーションを実装する。
- データアクセス ヘルパーに入力検証とエラー処理を追加する。

## まとめ

Cloud agent ハーネスを完了しました。これらのレッスンを通じて、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認**し、後で cloud agent の出力にどのように影響するかを把握しました。
- **Copilot cloud agent に issue を割り当て**、環境の準備、計画、非同期の実行を確認しました。
- アクセシビリティ向けの**カスタム エージェントを作成して使用**し、ハイ コントラストとライト モードの切り替えを追加しました。
- Copilot agents ページをミッション コントロールとして使い、作業中のアクセシビリティ セッションを監視してガイドしました。
- Copilot が作成したプルリクエストをレビューして反復し、**@copilot** をタグ付けして変更を依頼し、ワークフローを承認しました。

## レビューと次のステップ

Cloud agent ハーネスを完了しました。さらに学びたい場合は、ほかのハーネスで関連する内容を学べます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から直接 Copilot Chat のエージェント モードと MCP 統合を試します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan モードと Autopilot モード、エージェント スキル、カスタム QA エージェント、Playwright MCP、Agent Merge、cloud agent に戻るオプションの `/delegate` を使い、ターミナルから 2 つの変更をレビューしてリリースします。

自分のリポジトリでは、次のアイデアを試してみてください。

- リファクタリングやテスト範囲の issue を cloud agent に割り当てる。
- 別の分野のカスタム エージェントを作成する。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定する。

## リソース

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]
- [Copilot cloud agent のファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの監視と管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
