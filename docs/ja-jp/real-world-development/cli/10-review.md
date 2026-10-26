---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2 つのプルリクエスト、再利用可能なカスタマイズ、今後の参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot CLI を使って、Tailspin Toys の一連のワークフローに取り組みました。次のことを行いました。

- Codespace を準備し、Copilot CLI をインストールしてプロジェクトを調べ、用意されているフィルターの issue を確認しました。
- 星評価を追加し、転送したブラウザーで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- フィルターの issue から始めて、Plan モードで方針を定義し、Autopilot モードで作成して、Interactive モードでレビューしました。
- カスタム指示でエージェントをガイドし、既存の `quality-checks` スキルをカスタマイズして、ユニット テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーでフィルター機能を確認しました。
- 品質保証 (QA) カスタム エージェントを作成して選択し、要件、テスト範囲、スキルの結果、ブラウザーでの確認結果を評価しました。
- フィルターの変更全体をレビューし、フィルター PR に対して Agent Merge を許可しました。
- コンテキスト、モデル、共有、オプションのクラウド委任に使うスラッシュ コマンドを確認しました。

## リリースした内容

このワークショップには、更新済みの `main` から作成したそれぞれのブランチで、2 つの PR の節目があります。

1. **星評価:** ゲーム カードに既存の `starRating` と評価がない場合の明示的な表示を追加しました。
2. **フィルターと品質ワークフロー:** フィルターを実装し、指示を更新して機能に適用し、`quality-checks` のレポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加しました。

フィルターの計画から PR の作成までは、同じ会話とブランチを使いました。ワークショップを簡潔にするため、それらの作業を 1 つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP を使った Copilot のブラウザー操作という複数の方法でコードを確認しました。`quality-checks` スキルは、ユニット テスト、lint、型チェックを実行し、新しい形式で結果を報告しました。QA はこれらの結果をまとめて、要件とテスト範囲のレビューも行いました。

追加するテストは、実際に不足している範囲を補うものにしてください。新しいテストが不要という QA の結果が正しい場合もあります。マージを許可する前にコードと根拠を確認し、変更後は影響を受ける検証結果を更新してください。

## ベスト プラクティス

Copilot に提供するコンテキストやツールは、作業内容に影響します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズを会話間で再利用し、チームのニーズに応じて調整してください。指示は標準を設定し、スキルは繰り返し行う作業を説明し、カスタム エージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果をレビューしてください。

タスクに合った**モードとモデル**を選びます。構築前に方針を検討するには **Plan**、範囲を絞った変更でやり取りを続けるには **Interactive**、明確に範囲を定めたタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業には高性能なモデルを選択します。

インフラストラクチャと同じくらいコンテキストも重要です。作成したいもの、その理由、方法を明確に説明すると、出力が大きく変わります。

## さらに試す

基本的なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビュー エージェントに変更を分析してもらいます。
- `/rubber-duck` で問題について相談し、別の視点を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot cloud agent にタスクを送信します。

## 次のステップ

ツールの使い方を上達させるには、使い続けるのが一番です。業務コード、趣味のコード、何年も作りたいと思っていた小さなアプリなどで活用してください。チームで学びを共有し、お互いの経験から学びましょう。そして、ドキュメントも引き続き確認してください。

ターミナルから Tailspin Toys をさらに拡張するには、オプションの [Foundry Backer Concierge シリーズ][foundry]に進みます。ほかの環境と比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot app ワークショップ][app]、[Copilot cloud agent ワークショップ][cloud]を確認してください。

## リソース

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンド リファレンス][cli-reference]
- [Copilot CLI をカスタマイズする][customize-cli]
- [Copilot CLI でプルリクエストを管理する][manage-prs]

[previous-lesson]: ../9-cli-power-tools/
[foundry]: ../8-foundry-agent/
[vscode]: ../../vscode/
[app]: ../../app/
[cloud]: ../../cloud/
[about-cli]: https://docs.github.com/copilot/concepts/agents/about-copilot-cli
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
[customize-cli]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
