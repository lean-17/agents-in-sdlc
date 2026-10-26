---
title: "演習 4 - エージェントの監視と管理"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

ここまでの演習では、ユーザー エクスペリエンスの改善と機能追加を目的に、Copilot cloud agent に 3 つのタスクを依頼しました。cloud agent は非同期かつ自律的に動作しますが、作業の監視も重要です。

cloud agent に割り当てたタスクを管理する方法はいくつもあります。その 1 つが GitHub.com の[エージェント ページ][agents-page]です。このミッション コントロールでは、オープンなプルリクエスト (PR) があるすべてのエージェント タスクを確認できます。実行内容を調べたり、進行中のセッションにガイダンスを送ったりできます。

このレッスンでは、次のことを行います。

- エージェント ページで cloud agent のタスクを監視します。
- 進行中のセッションに追加機能を依頼します。

## シナリオ

エージェントにハイ コントラスト モードの作成を依頼した後、チームはライト モードも追加することにしました。サイトのスタイルを更新して切り替え機能を追加する作業がすでに進んでいるため、同じタイミングで実装するのが合理的です。ハイ コントラスト モードだけでなくライト モードも追加されるよう、エージェントの作業をガイドします。

## Copilot cloud agent のタスクを確認する

Copilot cloud agent に割り当てたすべてのタスクの現在の状態を確認します。

1. [Copilot agents ページ](https://github.com/copilot/agents)に移動します。
2. メイン ペインと左ペインにあるタスクの一覧を確認します。割り当てたタスクが表示されます。次のタスクが含まれているはずです。
    - コードのドキュメントを更新する
    - ゲームを変更するための API を生成する
    - Web サイトにハイ コントラスト モードを追加する
3. 実行中のタスクを 1 つ選択します。Copilot が実行した作業を確認します。次のような作業が表示されます。
    - リポジトリからコードをチェックアウトする
    - Copilot の作業環境を作成する
    - MCP サーバーを設定する
    - 割り当てられたタスクを完了するための手順を実行する

> [!NOTE]
> 表示される正確な手順は、Copilot の作業状況や採用した方法によって異なります。

4. 右側に表示されるプルリクエスト (PR) ペインも確認します。PR と変更されたファイルを確認し、作業の進捗を監視できます。

## Cloud agent の作業をガイドする

実行中のタスクを確認できたので、ハイ コントラスト モードの作業中にライト モードの切り替え機能も追加するよう Copilot に依頼します。

1. ハイ コントラスト モードを追加するセッションを選択します。正確なタイトルは、Copilot が付けた名前と作業状況によって異なります。

    ![ミッション コントロールのアクセシビリティ セッション](../../../_images/ex6-accessibility-session.png)

2. セッションを数分間確認し、セットアップが終わって作業を開始したことを示すまで待ちます。次のようなメッセージが表示されれば、作業を開始しています。
3. **Steer active session while Copilot is working** ダイアログに、次のプロンプトを入力します。

    ```plaintext
    While we are working on a high contrast mode, let's also add a light mode. There should be a switch for this mode as well where users can select their desired display mode.
    ```

    ![Copilot が作業中のエージェント ページで、Steer active session while Copilot is working ダイアログを強調表示したスクリーンショット](../../../_images/ex6-steer-cloud-agent-task.png)

4. <kbd>Enter</kbd> を押してプロンプトを送信します。
5. Copilot がプロンプトを認識し、作業に取り込む様子を確認します。

## Copilot に作業を続けてもらう

先ほどと同じように、Copilot が更新されたタスクに取り組みます。メッセージを送信した時点で進行中の手順が完了すると、新しい依頼を作業に取り込みます。

今回も数分かかるので、これまでに学んだことを振り返りながら待ちましょう。

## まとめと次のステップ

このレッスンでは、GitHub Copilot cloud agent のタスクを監視してガイドするための中心的な場所である Copilot agents ページを確認しました。このミッション コントロールでは、進行中や完了済みのタスクを追跡し、実行中の作業をレビューして、進行中のタスクの範囲を調整したり追加のガイダンスを提供したりできます。

次のことを行いました。

- ミッション コントロールとエージェント ページで cloud agent のタスクを監視しました。
- 進行中のセッションに追加機能を依頼しました。

Copilot がアクセシビリティ機能の作業を進めている間に、Copilot が作成したプルリクエストを[反復して改善][next-lesson]しましょう。

## リソース

- [Copilot agents ページ][agents-page]
- [カスタム エージェントについて][custom-agents]

---

| [← 前のレッスン: カスタム エージェント][previous-lesson] | [次のレッスン: Copilot の作業を反復する →][next-lesson] |
|:--|--:|

[previous-lesson]: ../3-custom-agents/
[next-lesson]: ../5-iterating/
[agents-page]: https://github.blog/changelog/2025-07-03-agents-page-for-copilot-coding-agent-in-public-preview
[custom-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-custom-agents
