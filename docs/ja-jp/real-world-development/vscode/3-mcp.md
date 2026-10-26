---
title: "演習 3 - Playwright MCP サーバーで機能をテストする"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: エージェント モード][previous-lesson] · [次のレッスン: カスタム エージェント →][next-lesson]

エージェント モードでフィルター機能を作成しました。プルリクエストを開く前に、ブラウザーで実際に動作することを確認します。自分でアプリを操作する代わりに、**Playwright MCP サーバー**を接続して、Copilot に実際のブラウザーを操作させて機能をテストします。その後、ブランチを公開して PR を作成します。

この演習では、次のことを行います。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを提供する方法を学びます。
- Playwright MCP サーバーをワークスペースに追加します。
- Copilot にサーバーを使わせて、ブラウザーでフィルター機能を手動テストします。
- ブランチを公開し、フィルター作業のプルリクエストを作成します。

## Model Context Protocol (MCP) とは

エージェント モードは、エディターの外にアクセスできるとさらに便利になります。Copilot は Model Context Protocol (MCP) を使って外部のツールやサービスと通信します。MCP は、エージェントが外部のツールやサービスとやり取りするための標準です。

![エージェント モードがコンテキスト、LLM、MCP サーバーや VS Code 拡張機能が提供するツールとどのように連携するかを示す図](../../../_images/ex1-mcp-diagram.png)

[Model Context Protocol (MCP)](https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/) を使うと、AI エージェントは外部のツールやサービスと通信できます。MCP によって、AI エージェントはリアルタイムに通信し、リソースから最新情報を取得したり、ツールを使ってユーザーに代わって操作を実行したりできます。

これらのツールやリソースには、AI エージェントと外部のツールやサービスをつなぐ MCP サーバーを介してアクセスします。MCP サーバーは、既存の API や NPM パッケージなどのローカル ツールとの通信を管理します。各 MCP サーバーは、AI エージェントが利用できるツールやリソースを提供します。

よく使われる MCP サーバーには、次のものがあります。

- **[GitHub MCP Server](https://github.com/github/github-mcp-server)**: GitHub リポジトリを管理する API を提供します。リポジトリの作成と更新、issue やプルリクエストの管理などを AI エージェントが実行できます。
- **[Playwright MCP Server](https://github.com/microsoft/playwright-mcp)**: Playwright を使ったブラウザーの自動操作を提供します。Web ページへの移動、フォームへの入力、ボタンの選択などを AI エージェントが実行できます。

ほかにも、さまざまなツールやリソースにアクセスできる MCP サーバーがあります。GitHub は、MCP サーバーを見つけやすくし、エコシステムへの貢献を促すために [MCP registry](https://github.com/mcp) をホストしています。

> [!CAUTION]
> セキュリティの観点から、MCP サーバーはプロジェクトのほかの依存関係と同じように扱ってください。使用前にソース コードを慎重に確認し、公開元を検証して、セキュリティへの影響を検討します。信頼できる MCP サーバーのみを使い、機密リソースや操作へのアクセスを許可するときは注意してください。

## MCP の構成を確認する

`.vscode/mcp.json` ファイルでは、この VS Code ワークスペースで利用できる MCP サーバーを構成します。

1. codespace で `.vscode/mcp.json` を開きます。
2. `github` サーバーがすでに構成されています。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        }
      }
    }
    ```

この `github` エントリはプロジェクト テンプレートに含まれているため、Copilot は前の演習でバックログの issue を読み取れました。これは[リモート GitHub MCP サーバー][remote-github-mcp-server]を使います。ローカルへのインストールは不要です。VS Code が HTTP 経由で接続し、Copilot がツールを初めて使うときに GitHub で認証します。

## Playwright MCP サーバーを追加する

次に 2 つ目のサーバーを追加します。[Playwright MCP サーバー][playwright-mcp-server]は Copilot が操作できるブラウザーを提供します。機能のテストに利用できます。

1. `.vscode/mcp.json` の `github` と並ぶ位置に `playwright` エントリを追加し、次のようにします。

    ```json
    {
      "servers": {
        "github": {
          "type": "http",
          "url": "https://api.githubcopilot.com/mcp/"
        },
        "playwright": {
          "command": "npx",
          "args": ["@playwright/mcp@latest", "--headless"]
        }
      }
    }
    ```

2. ファイルを保存します。

リモートの `github` サーバーとは異なり、`playwright` は**ローカル**サーバーです。VS Code は `npx @playwright/mcp@latest` を実行してサーバーを起動します。`--headless` は表示ウィンドウなしでブラウザーを実行するオプションです。デスクトップ画面がない codespace では、この設定が必要です。

> [!NOTE]
> Tailspin Toys プロジェクトはエンドツーエンド テストに Playwright を使っているため、ブラウザーはすでにインストールされていることが多いです。Copilot がブラウザーの不足を報告した場合は、`npx playwright install chromium` を実行してから再試行してください。

## サーバーを起動して信頼する

VS Code は MCP サーバーを**必要に応じて**起動します。Copilot がそのサーバーのツールを初めて必要としたときに起動し、信頼するか確認します。

1. `.vscode/mcp.json` で `playwright` エントリの上に **Start** アクションが表示されます。選択してサーバーを起動し、構成が正しいことを確認します。
2. VS Code がサーバーを信頼するか尋ねたら、構成を確認して信頼を選択し、サーバーを起動できるようにします。

手動での起動は省略できます。その場合は、次の手順で Copilot がツールを初めて必要としたときに VS Code がサーバーを起動し、信頼を確認します。

## フィルター機能をテストする

[Playwright MCP サーバー][playwright-mcp-server]を使うと、Copilot が実際のブラウザーを操作できます。自分でアプリを操作して変更を確認する代わりに、エージェントがページを開き、移動し、フィルターを適用して、結果を読み取って要約します。会話を離れずに機能が期待どおりに動作するかをすばやく確認できます。

Playwright MCP サーバーは、スクリーンショットではなくページの[アクセシビリティ ツリー][playwright-mcp-server]を使って動作します。つまり、エージェントはボタン、リンク、リスト項目などの構造化されたラベル付き要素を使って判断します。これは支援技術の動作と似ているため、機能確認と簡単なアクセシビリティ確認を同時に行えます。

サーバーを接続してアプリを起動したら、次のプロンプトで作成したフィルター機能を試すよう Copilot に依頼します。

```text
Using the Playwright MCP server, open a browser to the running app at http://localhost:4321 and verify the new game filtering feature:

1. Go to the games page and note how many games are listed.
2. Apply a category filter and confirm the list updates to only show games in that category.
3. Clear it, then apply a publisher filter and confirm the list updates to that publisher.
4. Combine a category and a publisher filter and confirm the results respect both.

Report what you observe at each step, and call out anything that does not behave as expected.
```

Copilot は Playwright MCP サーバー経由でブラウザーを起動し、各手順を実行して結果を報告します。issue の受け入れ条件と照らして概要を確認してください。問題があれば、プルリクエストを開く前に追加の質問をしたり、コードの修正を依頼したりします。

> [!NOTE]
> このテストではアプリが `http://localhost:4321` で実行されている必要があります。開発サーバーを停止していた場合は、プロンプトを送る前に再起動してください。Copilot が Playwright MCP サーバーを初めて使うとき、ブラウザーをダウンロードする場合があります。不足が報告されたら `npx playwright install chromium` を実行して再試行してください。

## ブランチを公開してプルリクエストを作成する

機能が動作することを確認できたので、チームのレビュー用にプルリクエスト (PR) を作成します。最初に `filtering-vscode` ブランチを公開します。

1. codespace の **Source Control** パネルに移動し、Copilot が加えた変更を確認します。
2. **+** アイコンを選択して変更をステージします。
3. **Sparkle** ボタンを使ってコミット メッセージを生成します。

   ![変更を表示する Source Control パネルのスクリーンショット](../../../_images/ex3-source-control-changes.png)

4. **Publish** を選択してブランチをリポジトリにプッシュします。

## プルリクエストを作成する

プルリクエストは github.com や GitHub コマンドライン インターフェイス (CLI) などから作成できます。ここでは GitHub Copilot に作成してもらいましょう。関連する issue を見つけて PR に関連付けられます。

1. Copilot Chat パネルに移動し、**New Chat** を選択して新しいセッションを開始します。
2. エージェント ドロップダウンで **Agent** が選択されていることを確認します。Copilot が GitHub ツールを使えるようにします。
3. 次のプロンプトで PR の作成を依頼します。

   ```plaintext
   Find the issue in the repo related to filtering by category and publisher. Create a new pull request for the current branch, and associate it with the correct issue.
   ```

4. 必要に応じて **Continue** を選択し、情報の取得や操作を許可します。GitHub ツールを初めて使うときは GitHub への認証を求められる場合があります。画面の手順に従って許可してください。
5. Copilot が issue を検索し、正しい issue を見つけて PR を作成する様子を確認します。
6. Copilot が作成したリンクを選択してプルリクエストを確認します。ただし、**まだマージしないでください**。

## まとめと次のステップ

この演習では、次のことを行いました。

- Model Context Protocol (MCP) と、MCP サーバーが Copilot に新しいツールを提供する方法を学びました。
- Playwright MCP サーバーをワークスペースに追加しました。
- ブラウザーでフィルター機能を手動テストするために使用しました。
- ブランチを公開し、フィルター作業のプルリクエストを作成しました。

MCP サーバーを使って機能を**テスト**しましたが、MCP は Copilot を拡張する方法の 1 つにすぎません。次は、アクセシビリティ レビューなどの作業を効率化する[カスタム エージェント][next-lesson]を作成します。

## リソース

- [MCP とは何か、なぜ話題になっているのか][mcp-blog-post]
- [Microsoft Playwright MCP Server][playwright-mcp-server]
- [GitHub MCP Server][github-mcp-server]
- [GitHub MCP Registry][mcp-registry]
- [VS Code の MCP サーバー][vscode-mcp-config]

[previous-lesson]: ../2-agent-mode/
[next-lesson]: ../4-custom-agents/
[mcp-blog-post]: https://github.blog/ai-and-ml/llms/what-the-heck-is-mcp-and-why-is-everyone-talking-about-it/
[github-mcp-server]: https://github.com/github/github-mcp-server
[mcp-registry]: https://github.com/mcp
[remote-github-mcp-server]: https://github.blog/changelog/2025-06-12-remote-github-mcp-server-is-now-available-in-public-preview/
[vscode-mcp-config]: https://code.visualstudio.com/docs/agents/reference/mcp-configuration
