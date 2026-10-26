---
title: "レッスン 4 - カスタム指示で Copilot をガイドする"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、フィルターのコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。タスクを特定の方法で実行してほしい場合は、そのガイダンスを Copilot が参照できるようにします。[指示ファイル][instruction-files]では、作成するコードだけでなく、その構成方法も指定できます。フィルターを実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加してコードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスを対象とする指示ファイルがエージェントに適用される仕組みを確認します。
- コーディング標準を守るために指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys のような開発会社には、開発方法に関するガイドラインや要件があります。たとえば、次のようなものです。

- コメントは、コードを言い換えるのではなく、意図や自明でない決定を説明する必要があります。
- `db/` と `src/lib/` のエクスポートされた関数では、`db` 引数を注入できる場合はその引数も含め、目的、パラメーター、戻り値を TSDoc/JSDoc で説明します。
- 再利用可能な Astro コンポーネントでは `Props` の契約を説明し、関連コードが変更されたらコメントも最新の状態に保ちます。
- 既存のフォーマットと lint のガイダンスを維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業するために必要な情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストや希望を Copilot に伝えられます。好みのコーディング規約、ライブラリ、コメントの形式などを指定できるため、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体に適用する指示と、特定のファイルやタスクに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。ほとんどのリクエストに関係するプロジェクト全体のコンテキストを記述します。
- `.github/instructions/*.instructions.md` ファイルには、特定の言語、ファイルの種類、タスクに対するガイドラインを記述します。

> [!NOTE]
> ほかの指示形式やサポート状況は環境によって異なります。特定の形式を使う前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## このプロジェクトのカスタム指示ファイルを確認する

スターター プロジェクトには、作業を始めやすいように指示ファイルが用意されています。変更を加える前に内容を確認し、影響を見てみましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング ガイダンスを確認します。この指示は、リポジトリで Copilot を使うすべてのやり取りに適用されます。
4. `.github/instructions` フォルダーを開き、Astro ファイル、Drizzle データ層、テストなどに関する指示を確認します。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドで、指示の適用対象となるファイルを glob で指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、ほかの指示ファイルや既存ファイルへのリンクを確認します。これにより、大きな指示を再利用可能な小さなファイルに分け、Copilot に参考例を示せます。

## チームのガイダンスに合わせて指示ファイルを更新する

用意されているファイルはよい出発点ですが、まだ不足があります。コアの `copilot-instructions.md` を変更して、新しく生成する TypeScript に TSDoc コメントを追加するよう指定します。

1. `.github/copilot-instructions.md` の **Code formatting guidance** セクションを、35 行目付近で見つけます。
2. 次の内容を最後の箇条書きとして追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新したガイダンスを適用する

Copilot CLI は会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、終了してからフィルターの会話を再開します。これにより、機能のコンテキストを維持したまま、新しいガイダンスを読み込めます。

1. フィルターの会話を続けている場合は `/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. `game-filters-cli` ブランチ上のリポジトリ ルートから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   プロンプトが表示されたらフィルターの会話を選択し、`game-filters-cli` ブランチがチェックアウトされていることを確認します。

3. Copilot に更新したガイダンスを適用するよう依頼します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力して、変更された TypeScript ファイルを確認します。新しい TSDoc コメントがコードを正確に説明していることを確かめます。

## まとめと次のステップ

Copilot が指示ファイルからコンテキストを取得する仕組みを確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスを対象とする指示ファイルがエージェントに適用される仕組みを確認しました。
- 指示ファイルを更新してコーディング標準を設定しました。
- 指示ファイルがコードに与える影響を確認しました。

次は再利用可能な `quality-checks` スキルを[カスタマイズして実行][next-lesson]し、lint とテストを一貫して実行できるようにします。

## リソース

- [Copilot CLI にカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
