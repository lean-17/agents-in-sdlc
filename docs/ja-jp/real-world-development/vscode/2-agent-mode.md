---
title: "演習 2 - Copilot エージェント モードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

アプリケーションに簡単な更新を加えるだけでも、通常は複数のファイルを変更してテストを実行する必要があります。開発者は必要なファイルを見つけて変更し、テストを実行し、デバッグして、変更し忘れたファイルを見つけて更新する、といった作業を繰り返します。

このような作業を支援するのが Copilot エージェント モードです。

Copilot エージェント モードは、IDE でより自律的に動作するよう設計されています。開発者と同じように既存のプロジェクト構成を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェント モードでサイトに新機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェント モードと cloud agent は異なる用途向けです。エージェント モードは IDE 内で作業し、すばやくフィードバックを受けてやり取りできます。Cloud agent はチームのメンバーのように非同期で動作し、issue とプルリクエストを通じてやり取りします。

この演習では、次のことを学びます。

- Copilot エージェント モードがプロジェクトを調べ、関連ファイルを特定し、複数の変更を連携して行う方法
- GitHub Copilot エージェント モードで UI とデータ層にまたがる機能を実装する方法
- コードベースにマージする前に、Copilot エージェント モードが生成した変更とテストをレビューする方法

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリや発行元で絞り込めるようにします。前の演習で発行元ヘルパーを追加したので、残りのデータ層、UI、テストを Copilot エージェント モードで仕上げます。

## Tailspin Toys Web サイトを実行する

変更を加える前に Tailspin Toys Web サイトを確認し、現在の機能を把握しましょう。

この Web サイトは、開発者をテーマにしたボードゲームのクラウドファンディング プラットフォームです。ゲームの一覧と詳細を表示できます。単一の Astro アプリであり、ビルド時に静的 HTML としてページをレンダリングします。ページは frontmatter から Drizzle ORM を使ってローカルの SQLite データベースに直接問い合わせます。独立したバックエンド API やクライアント側 UI フレームワークはありません。再利用可能なデータアクセス ヘルパーは `src/lib/` にあり、インタラクティブな機能には標準 DOM API を使う小さなスコープ付き Astro `<script>` を使います。

### Web サイトを起動する

Web サイトを簡単に実行できるよう、単一の Astro 開発サーバーを起動する `npm run dev` タスクが用意されています。GitHub Codespace で次の手順を実行します。

1. 前の演習の後に `main` に戻っていることを確認して、codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、codespace 内に新しいターミナルを開きます。
3. フィルター作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すバナーが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` をブラウザーで開くか確認するダイアログが表示された場合は、**x** を選択して閉じてください。

5. <kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら、ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を選択して Web サイトを開きます。

> [!NOTE]
> codespace では、ターミナルに表示された localhost URL を選択すると、`https://<your-codespace-name>-4321.app.github.dev/` に自動的に転送されます。これは Web サーバーをホストしている codespace へのプライベートなトンネルです。

### Web サイトを確認する

Web サイトを起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホーム ページ**: ボードゲームの一覧、タイトル、画像、説明を表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、発行元、カテゴリなどの情報を表示します。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な手順は各セクションで説明します。

Web サイトの初期実装は動作しますが、新しい機能を追加して改善したいと考えています。まずバックログを確認しましょう。テンプレートからプロジェクトを設定すると、issue のバックログが自動で作成されます。GitHub Copilot に一覧を表示してもらいます。

1. Chat ビューのエージェント ドロップダウンから **Agent** を選択します。**Agent** エージェントは、複数ファイルの変更を自律的に計画して実装し、ターミナル コマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルから **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> このワークショップの作成者は、特定のモデルを推奨しているわけではありません。作成時に Claude Sonnet 4.5 を使ったため、同じような体験を得やすいよう手順に記載しています。LLM は確率的に動作するため、記載内容とは異なる提案が表示される場合があります。これは正常であり、想定内です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の MCP コマンドを使う場合がありますが、それでもタスクを完了できるはずです。

3. 次のプロンプトを Copilot に送信し、issue のバックログを確認します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. **Continue** を選択して、すべての issue を一覧表示するコマンドを実行します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと考える項目を優先順位付けしたことも確認できます。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを提供する指示ファイルを確認しましょう。[ユーザー インターフェイス (UI) の指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)には、Web サイトに機能を追加する方法が記載されています。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の方法に関する全般的なガイダンスを確認します。次の内容が含まれています。
   - アーキテクチャの概要
   - コンポーネント設計、テスト容易性、アクセシビリティの原則
   - Astro、Tailwind CSS、Drizzle など、ファイルの種類ごとの指示へのリンク

> [!TIP]
> 指示ファイルから、ほかの指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルからの相対パスです。指示の再利用、複雑な指示の分割、参考例やテンプレートの提示に役立ちます。

## フィルター機能を実装する

フィルター機能を完成させるには、少なくとも次の 3 つの変更が必要です。

- データ層 (`src/lib/`) に残りのフィルター処理を追加または改善する
- フィルターの動作をテストするテストを追加または更新する
- ゲーム一覧ページにフィルター UI を追加する

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェント モードにこれらの作業を依頼しましょう。

1. 現在の Copilot 会話を続けるか、**New Chat** を選択して新しい会話を開始します。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューのエージェント ドロップダウンで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. モデルが引き続き **Claude Sonnet 4.5** であることを確認します。
5. バックログにある関連 issue の要件に基づいて実装するよう、次のプロンプトで Copilot に依頼します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、必要な機能に関係するファイルを見つける様子を確認します。データ層のヘルパー、UI、テストを見つけてから、ファイルを変更しテストを実行します。

   ![Copilot がプロジェクト ファイルを調べる様子](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行など、複数の作業を行います。タスクやコードベースの複雑さによっては数分かかります。作業中にエディターに **Keep** と **Undo** が表示される場合があります。完了するとすべての変更に **Keep** または **Undo** が表示されるため、作業中に選択する必要はありません。

7. Copilot から求められたら **Continue** を選択してテストを実行します。

   ![Copilot Chat でテスト実行の確認を求めるダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、一部のテストが失敗したりすることがあります。問題ありません。Copilot はタスクが完了し、エラーがなくなるまで、コード生成とテストを繰り返します。

   ![Copilot エージェント モードの完了した Chat セッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ずレビューしてください。

10. Web サイトを開いているブラウザーに戻り、新機能を確認します。
11. 動作を確認してコードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

GitHub Copilot エージェント モードを使って Tailspin Toys Web サイトに新機能を追加しました。次の方法を確認しました。

- GitHub Copilot エージェント モードで UI とデータ層にまたがる機能を実装する
- プロジェクトを調べ、関連ファイルを見つけ、複数の変更を連携して行う
- コードベースにマージする前に、生成された変更とテストをレビューする

次は [Playwright MCP サーバーで機能をテスト][next-lesson]し、プルリクエストを作成します。

### おまけの演習 — ページネーションを実装する

ゲームの一覧が増えると、ページネーションが必要になります。この演習で学んだスキルを使って、Copilot にページネーションの実装を依頼してみましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、現在のベスト プラクティスに従う。
- ユーザーがページ サイズを選択できるようにするか、固定値にするかを検討する。
- 希望する実装に必要なガイダンスをプロンプトで Copilot に伝える。
- Copilot にコンテキストを追加したり変更を依頼したりして、反復が必要になることがある。これは Copilot を使う通常の作業方法です。

## リソース

- [Copilot の ask、edit、agent モード: それぞれの役割と使い分け][choose-mode]
- [VS Code のエージェント モード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
