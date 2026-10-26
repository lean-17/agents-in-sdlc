---
title: "レッスン 3 - エージェント モード: Plan と Autopilot"
description: "Plan で方針を決め、Autopilot で issue に基づくフィルターを実装し、Interactive モードで結果をレビューします。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

最初にプロジェクトへ小さな機能を追加しました。より大きな変更には、さらにしっかりしたプロセスが必要です。GitHub Copilot CLI は組織の既存の開発フローに沿って動作するため、適切な方法で必要なものを構築できます。このレッスンから、エージェントを活用する一般的な開発プロセスをいくつかのレッスンにわたって進めます。まず issue から機能を作成し、コードが妥当であることと機能が期待どおりに動作することを確認して、最後にプロジェクトへマージします。

> [!NOTE]
> 機能のワークフローを続ける間は、同じ会話とブランチを使います。通常はファイルの種類ごとにブランチや PR を分けますが、ここでは重要な概念に集中するため簡略化しています。

このレッスンでは、次のことを行います。

- GitHub issue から Copilot との新しい会話を開始します。
- Plan モードで要件を定義します。
- Autopilot モードで新機能を実装します。
- コードをレビューします。
- 転送したブラウザーで機能を手動検証します。

この機能の作業を続けながら、リポジトリの指示を更新し、既存の `quality-checks` スキルをカスタマイズし、MCP による検証を追加し、QA エージェントを作成して、機能 PR を開きます。

## シナリオ

Tailspin Toys のカタログが増え、訪問者がカテゴリや発行元でゲームを絞り込めるようにする必要があります。バックログの issue に機能の概要はありますが、カテゴリを組み合わせる方法など、コーディング前に合意が必要な詳細があります。Plan モードで決定し、範囲を限定した実装を Autopilot に任せます。

## 背景

AI コーディング エージェントを開発フローに導入しても、基本は変わりません。むしろ、これまで以上に重要になります。多くの開発者は、次のような手順で作業します。

1. 実装内容を記述した issue から始めます。
2. 作成するものの計画を立てます。
3. コードを作成してレビューします。
4. テストを実行してコードを検証します。
5. 新機能を手動で検証します。
6. プルリクエスト (PR) を作成します。
7. コードレビューと継続的インテグレーションが成功した後、コードをマージします。

> [!NOTE]
> 詳細はチームや組織によって異なりますが、多くの場合は上記の流れを基本とします。

この標準的な方法を守ることで、AI が生成したコードも要件を満たし、手書きのコードと同じ審査プロセスを経るようにできます。

## 会話モード

**会話モード**は、エージェントにどの程度の自律性を与えるかを制御します。<kbd>Shift</kbd>+<kbd>Tab</kbd> を押すとモードを切り替えられます。

- **Interactive**: エージェントと共同作業します。エージェントは変更を提案し、続行する前に入力を待ちます。
- **Plan**: エージェントが最初に計画を作成します。プロジェクト ファイルの編集はできません。
- **Autopilot**: エージェントが自律的にコードを作成し、テストを実行して、タスクが完了するまで反復します。

まず Plan モードで計画を確認し、その後 Autopilot で実装します。

## issue から始める

フィルターを実装する前に、Codespace に戻り、リポジトリとターミナルの準備ができていることを確認します。

1. Codespace に戻ります。停止している場合は、再起動してから続けます。
2. 星評価の PR がマージされていることを確認します。
3. ターミナルが開いていない場合は、<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
4. `main` を更新してから、フィルター用のブランチを作成します。

   ```bash
   git checkout main
   git pull --ff-only
   git checkout -b game-filters-cli
   ```

5. Copilot CLI を起動します。

   ```bash
   copilot --yolo
   ```

6. <kbd>Tab</kbd> を 2 回押して **Issues** タブを開きます。
7. <kbd>A</kbd> を押してすべての issue を表示します。
8. 矢印キーで **Allow users to filter games by category and publisher** というタイトルの issue を選択します。
9. <kbd>C</kbd> を押して issue をプロンプトに追加し、**Session** タブに戻ります。

プロンプトの先頭に `#7` (または同様の番号) が表示されます。`#` を使うと、GitHub 上の issue やプルリクエスト (PR) をコンテキストに含められます。

## フィルター機能を計画する

計画を立てると、Copilot に作業を依頼する前に機能の実装方法やタスクの進め方を決められます。複雑な作業では、計画に時間をかけることをお勧めします。Plan モードに切り替えて、計画の作成を依頼しましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Plan モードに切り替えます。プロンプトの下にあるモード表示が **Plan** になっていることを確認します。
2. 前の手順で追加した issue 参照の後に、次のプロンプトを入力します。

   ```plaintext
   Create a plan for implementing this feature.
   ```

Copilot はプロジェクトを調べて実装方法を検討し、計画を作成します。

3. フィルターの動作について質問されたら、希望に基づいて回答します。唯一の正解はありません。
4. 計画が完成したら、<kbd>Control</kbd>+<kbd>E</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>E</kbd> (Windows/Linux) を押して計画を展開します。
5. 計画を確認します。
6. 決定と合わない部分があれば、Copilot に修正を依頼します。

## Autopilot を承認する

計画を作成してレビューしたので、実装を開始します。Autopilot モードで Copilot に作業を任せましょう。

Autopilot は、タスクが完了したと判断するまで、Copilot が問題に取り組むことを可能にします。

1. **Accept plan and build on autopilot (recommended)**、またはインストール済みのバージョンで同様の名前が付いた選択肢を選びます。
2. プロンプトの下にあるモード表示が **Autopilot** になっていることを確認します。
3. Copilot が計画に沿って反復し、コードを生成してテストを実行する様子を確認します。

> [!NOTE]
> 承認するとすぐに実装が始まることがあります。先に計画を確認してください。Copilot が依存関係の不足やポートの競合を報告した場合は、チェックが完了したと判断する前にセットアップの問題を解決してください。

## 実装をレビューして検証する

生成されたコードは、ほかのコードと同じようにマージ前にレビューします。コードを確認し、サイトを実行して問題がないことを確かめましょう。

1. <kbd>Shift</kbd>+<kbd>Tab</kbd> を押して Interactive モードに切り替えます。モード表示が **Plan** や **Autopilot** ではないことを確認します。
2. `/diff` を入力して、フィルターの実装とテストを確認します。
3. issue と計画時に決めた内容に照らして結果を確認します。
4. レビューが終わったら、<kbd>Esc</kbd> を押して差分表示を閉じます。
5. プロジェクトのチェック結果を確認し、失敗があれば Copilot に修正を依頼します。

## 新機能を確認する

コードはよさそうですが、実際に動作するでしょうか。前と同じようにアプリを起動し、Codespaces の転送ポートからサイトを開きます。

1. Copilot にアプリの起動を依頼します。

   ```plaintext
   Start the app so I can try the filtering feature in my browser. Tell me the URL and leave the server running.
   ```

2. Codespaces でポート `4321` が利用可能と表示されたら、**Open in Browser** を選択します。
3. 計画で決めたとおりに、カテゴリ、発行元、および組み合わせのフィルターを試します。
4. リセットと検索結果がない場合の動作が、issue と決定内容に合っていることを確認します。
5. Codespace に戻り、Copilot に起動した開発サーバーの停止を依頼します。

## まとめと次のステップ

異なる会話モードを使って機能を作成し、レビューしました。このレッスンでは次のことを行いました。

- GitHub issue から Copilot との会話を開始しました。
- Plan モードで要件を定義しました。
- Autopilot モードで新機能を実装しました。
- コードをレビューしました。
- 転送したブラウザーで機能を手動検証しました。

次は[カスタム指示][next-lesson]を使って、コードが文書化されたプラクティスに従うようにします。

## リソース

- [GitHub Copilot CLI の Autopilot][autopilot]
- [Copilot CLI コマンド リファレンス][cli-reference]

[autopilot]: https://docs.github.com/copilot/concepts/agents/copilot-cli/autopilot
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[previous-lesson]: ../2-add-star-rating/
[next-lesson]: ../4-custom-instructions/
