---
title: "演習 2 - GitHub Copilot cloud agent"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩まされていない組織は、ほとんどありません。未解決のセキュリティ問題、更新が必要なレガシー コード、実装する時間がなくバックログに残った機能リクエストなどがその例です。GitHub Copilot cloud agent は、コードの更新や機能追加などを自律的に実行できます。作業が終わると、人間の開発者がレビューできるドラフト PR を作成します。これにより、手間のかかる作業を任せて開発を加速し、開発者はより大きな課題に集中できます。

Copilot cloud agent を使って、次のことを確認します。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 明確な範囲の issue が重要な理由
- Copilot への issue の割り当て

## シナリオ

Tailspin Toys には、解決したい技術的負債があります。最初のサイトを開発した業務委託チームは、ドキュメントを十分に整備していませんでした。まず、アプリケーションのエクスポートされたすべての関数に TSDoc のドキュメント コメントを追加します。

また、デザイン チームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに同じカテゴリのほかのゲームを表示し、ユーザーが引き続き閲覧できるようにします。まだ完成度の高い実装は必要なく、UX の受け入れテストに使えるものがあれば十分です。これは現在の課題ですが、より優先度の高い issue もあります。

このような作業は後回しになりやすいため、Copilot cloud agent に割り当てるのに適しています。エージェントに非同期で作業させている間、開発者は別の作業を進め、完了後に戻って結果をレビューできます。

## GitHub Copilot cloud agent とは

[GitHub Copilot cloud agent](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent) は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。たとえば、[GitHub issue を Copilot に割り当てる](https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions)方法があります。割り当てると、Copilot は進捗を追跡するドラフト プルリクエストを作成し、環境を準備してタスクを開始します。実行中でも完了後でも、Copilot のセッションを詳しく確認できます。提案された解決策のレビュー準備が整うと、プルリクエストで通知されます。

## 適切な範囲の指示が重要

GitHub Copilot に魔法はありません。考える必要がなくなるわけではありません。一見簡単な操作でも、詳細を確認すると複雑なことがあります。[Copilot cloud agent に依頼する作業の範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペア プログラマーとして扱い、段階的に作業し、学び、試して、必要に応じて調整します。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

これまでの演習で、カスタム指示と Copilot のガイド方法を確認しました。カスタム指示を扱ったことがあれば `.github/copilot-instructions.md` の動作を確認済みですが、まだの場合はここで簡単に見ておきましょう。Copilot cloud agent に作業を割り当てる前に、このリポジトリに含まれる指示ファイルを読み取り専用で確認し、後でその影響を見つけられるようにします。

GitHub Web UI または起動中の codespace で、次のファイルを開きます。

- `.github/copilot-instructions.md` - **Code standards** セクション、特に TypeScript 規約と TSDoc ドキュメント コメントの要件を確認します。
- `.github/instructions/unit-tests.instructions.md` - `applyTo` フロントマターを確認します。このファイルは `**/*.test.ts` に一致するファイルに指示を適用します。

次のセクションで `Code lacks documentation` issue を cloud agent に割り当てたら、作成されるプルリクエストにある TSDoc コメント、TypeScript 規約、コメント ヘッダーを確認してください。これらは指示ファイルの内容に基づいています。後の演習で PR をレビューするときに、もう一度確認します。

## Copilot cloud agent の開発環境を設定する

誰がコードを書く場合でも、通常は作業環境の準備や、環境を適切な状態にするためのスクリプト実行が必要です。Copilot にタスクを割り当てる場合も同様です。Copilot はソフトウェア エンジニアと似た方法で作業します。

Cloud agent は作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップ ワークフロー][setup-workflow]を作成すると、作業開始前に必要な開発ツールや依存関係を利用できるようになります。このラボの進行と学習を助けるため、すでに構成されています。単一の Astro アプリに必要な Node.js、プロジェクトの依存関係、エンドツーエンド テスト用のブラウザー バイナリ、マイグレーションとシードを適用した SQLite データベースを Copilot が使えるようにします。

```yaml
name: "Copilot Setup Steps"

# リポジトリの「Actions」タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *この手順*に必要な最小限の権限を設定します。Copilot は操作用に独自のトークンを受け取ります。
    permissions:
      # 依存関係のインストールなどのためセットアップ手順でリポジトリをクローンする場合は、`contents: read` 権限が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストに必要なデータを用意するため、ローカルの SQLite データベースにマイグレーションとシードを適用します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローと同じ形式ですが、次の点に注目してください。

- `copilot-setup-steps` という 1 つのジョブが含まれます。このジョブは Copilot がプルリクエストの作業を始める前に GitHub Actions で実行されます。
- `workflow_dispatch` トリガーを使うと、リポジトリの **Actions** タブからワークフローを手動で実行できます。Copilot が実行するのを待たずに、ワークフローが正常に動作するかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は誰もが理解していますが、多くのプロジェクトでは情報が古かったり、ドキュメント自体が不足していたりします。このような技術的負債は放置されやすく、生産性が低下し、コードベースの保守や新しい開発者の参加が難しくなります。Copilot はドキュメント作成が得意なので、cloud agent に割り当てるのに適した issue です。バックグラウンドで必要なドキュメントを生成し、後の演習で結果をレビューします。

1. 新しいブラウザー タブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

    ```plaintext
    Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

    ![Copilot の割り当て詳細](../../../_images/ex4-assign-copilot-details.png)

10. **Pull Requests** タブを選択します。
11. 新しく作成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のようになります。PR が一覧に表示されない場合は、少し待ってからブラウザーを更新してください。
12. 数分後、Copilot が todo リストを作成します。

> [!NOTE]
> Copilot の todo リストが PR に表示されるまで数分かかることがあります。Copilot は前述のワークフローを実行して環境を作り、プロジェクトを分析して、タスクの進め方を検討しています。

13. リストと、Copilot が実行する予定のタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

    ![Copilot のセッション表示](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 更新されたインジケーターを表示するには、ウィンドウを更新する必要がある場合があります。

16. ライブ セッションをスクロールし、Copilot がどのように問題に取り組むか確認します。コードを調査して状態を理解し、適切な計画を考えて決定し、コードを作成する様子を確認できます。

処理には数分かかることがあります。Copilot cloud agent の主な目的は、タスクを非同期に実行して、ほかの作業に集中できるようにすることです。この機能を活用して、もう 1 つのタスクも cloud agent に割り当てます。その後、アプリに機能を追加する作業に移ります。

## ゲーム詳細ページに関連ゲームを追加する

GitHub Copilot cloud agent の大きな利点の 1 つは、作業を分担できることです。デザイン チーム向けに関連ゲームを追加する作業に時間がかからないとしても、その時間をほかの作業に使うことができます。Copilot cloud agent に割り当てましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して新しい issue ダイアログを開きます。
4. **Blank issue** を選択します。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

    ```markdown
    We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
    ```

7. **Create** を選択して issue を作成します。
8. 右側にある **Assign to Copilot** を選択して割り当てダイアログを開きます。

    ![Copilot を issue に割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、issue の最初のコメントに 👀 が表示され、Copilot が作業を開始したことが分かります。

![Copilot が issue に取り組んでいることを示す目の絵文字](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は 2 つ目の依頼にも取り組んでいます。Copilot cloud agent はソフトウェア エンジニアと同じように作業するため、常に監視する必要はありません。完了後にレビューできます。次はカスタム エージェントの作成と使用に進みましょう。

## まとめと次のステップ

このレッスンでは [GitHub Copilot cloud agent][copilot-agents]を確認しました。cloud agent に issue を割り当てると、非同期で作業できます。技術的負債への対応、新機能の作成、別のフレームワークへのコード移行などに活用できます。

次のことを確認しました。

- コード生成環境のカスタマイズ
- 操作を安全に行う方法
- 明確な範囲の issue が重要な理由
- Copilot への issue の割り当て

cloud agent がバックグラウンドで作業している間に、カスタム エージェントの作成と使用に進みます。[Copilot cloud agent は MCP サーバーも使用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## リソース

- [Copilot cloud agent について][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot cloud agent のセットアップ ワークフローに関するベスト プラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタム エージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
