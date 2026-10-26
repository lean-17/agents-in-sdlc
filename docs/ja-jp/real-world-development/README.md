---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を段階的に計画してコードを作成し、動作をテストし、プルリクエストをレビューし、エージェントを使って作業を管理します。

## シナリオ

ボードゲームのクラウドファンディングを開発者向けに提供する架空の企業、Tailspin Toys に新しく加わった開発者として作業します。チームのバックログには GitHub issue が用意されており、すぐに着手できます。機能追加にはフィルターやページネーションがあり、品質改善にはアクセシビリティやコーディング標準への対応があります。

## 作業環境を選択する

GitHub Copilot は、どの環境でも利用できます。開発方法に合った環境を選択してください。どのワークショップも専用のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタム エージェント、Playwright MCP、Agent Merge、実践的なスラッシュ コマンドを扱います。

### [GitHub Copilot app][app]

デスクトップ アプリでエージェント セッションを並行して実行し、セッション モードを切り替え、Canvas で共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot cloud agent][cloud]

GitHub issue を非同期エージェントに割り当て、作業をガイドし、進捗を監視して、エージェントが作成したプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに Copilot Chat のエージェント モード、MCP サーバー、カスタム エージェントを利用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
