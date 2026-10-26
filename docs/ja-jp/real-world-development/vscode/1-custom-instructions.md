---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェント モード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行してほしい場合や Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。Copilot に具体的なコンテキストを伝える方法はいくつかあります。その中心となる[指示ファイル][instruction-files]は、コード生成に関するガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを対象とする指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- 現在の指示に従って、フィルター機能の最初のデータ部分 (発行元ヘルパー) を生成します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。
- 同じプロンプトを再実行し、生成コードが新しい標準に従うことを確認します。
- 指示の更新とフィルター機能の一部を `main` にコミットし、次の演習で Copilot が更新済みのガイダンスを使えるようにします。

> [!CAUTION]
> 生成されたコードは、設定した標準に従わない場合があります。Copilot は確率的に動作します。この演習では、出力を一字一句同じにするのではなく、指示を更新した後の傾向の変化を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys のような開発会社には、開発方法に関するガイドラインや要件があります。たとえば、次のようなものです。

- データ層には常にユニット テストが必要です。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc のドキュメント コメントを使ってコードを文書化します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿って作業するために必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストや希望を Copilot に伝えられます。好みのコーディング規約、ライブラリ、コメントの形式などを指定できるため、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体に適用する指示と、特定のファイルやタスクに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。技術スタック、開発対象の概要、ベスト プラクティス、全般的なガイダンスなど、プロジェクト全体のコンテキストを記述します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類を対象に作成できます。TypeScript や Astro などの言語、UI コンポーネントやユニット テストの作成などのタスクに対する指示を記述できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することはこの演習の範囲外ですが、サンプル プロジェクトには代表的な方法が含まれています。要点は次のとおりです。

- `copilot-instructions.md` には、開発対象やプロジェクト構成、全般的なコーディング標準など、プロジェクト全体に関するガイダンスを記述します。
- `*.instructions.md` ファイルには、ユニット テスト、Astro コンポーネント、データ層など、特定のファイルの種類やタスクに関する指示を記述します。
- 自然な言葉を使い、明確なガイダンスを記述します。コードの良い例と悪い例を示します。

AI の使い方と同様に、指示ファイルの書き方に決まった方法はありません。プロジェクトで試しながら、自分に合った方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うプロジェクトでは、充実した指示ファイルを用意しましょう。このプロジェクトには、[UI の変更][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク向けのファイルがあります。
>
> Copilot に指示ファイルを作成してもらうこともできます。各環境で手順は異なります (たとえば VS Code では **Configure Chat → Generate Agent Instructions**、Copilot CLI では `/init`)。必要に応じて、利用中の環境のレッスンで説明します。
>
> テンプレートや出発点を探している場合は、指示ファイルやカスタム エージェントなどを集めたリポジトリ [awesome-copilot][awesome-copilot] をご覧ください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## このプロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを読んでみましょう。コアの `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのやり取り全般に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示の適用対象をリポジトリ ルートからの相対 glob で指定します。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. このプロジェクトのユニット テスト作成に固有の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや既存ファイルへのリンクがあります。これにより、大きな指示を再利用可能な小さなファイルに分け、Copilot に参考例を示せます (パスは指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内ドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## 変更用ブランチを作成する

ここではベスト プラクティスに沿って、変更用のブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 次の演習を始める前に、codespace に戻り、Copilot Chat パネルを開いて **New Chat** を選択し、新しい会話を開始します。モードとモデルの選択は演習ごとに異なります。必要な手順は各セクションで説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。その後、指示を更新して同じプロンプトを再送信し、違いを確認します。

1. 前の演習で開いたエディター タブを閉じ、Copilot が必要なコンテキストだけを取得するようにします。
2. `src/lib/publishers.ts` を開き、ヘルパーの配置場所を Copilot に伝えます。
3. Chat ビューのエージェント ドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択画面](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。ヘルパー ファイルとそのテストなど、複数のファイルに変更が加わることがあります。
6. 提案されたヘルパーが `db` クライアントを最初の引数に取り、発行元の型付き配列を返す型付き関数であることを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータ層規約に基づいています。
7. 提案されたコードに TSDoc コメントとファイル レベルのコメント ヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメント コメントを追加する可能性があります。その場合も問題ありません。指示を更新した後の一貫性の改善を確認することが重要です。

## リポジトリ標準を追加する

前述のように、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を記録しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。コーディング標準は記載されていますが、コード内ドキュメントのルールがありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加してファイル コメント ヘッダーと TSDoc コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` には全般的な指示を、`*.instructions.md` ファイルには言語、ファイルの種類、タスク固有の指示を記述できます。ドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## プロンプトを再実行して違いを確認する

1. Copilot Chat に戻り、**New Chat** を選択してバッファーをクリアします。
2. `src/lib/publishers.ts` をもう一度開き、Copilot が適切なファイルに注目するようにします。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメント ブロックが追加されたことを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントがあることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

Copilot に新しいプロジェクト標準を適用させ、次の演習で使用する実際のコードに反映できました。

## 指示とブランチをコミットしてプッシュする

指示ファイルもほかのアセットと同じように、ソース管理を使って管理します。指示の更新とヘルパーの変更をコミットして、ブランチをリポジトリにプッシュしましょう。

1. <kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示の更新とヘルパーの変更があることを確認します。

   ```bash
   git status
   ```

3. 指示の更新とヘルパーの作成で Copilot が変更したファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # If git status shows additional supporting updates (for example tests), add those files too.
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたので、バックログのドキュメント標準 issue に関連付けた PR を作成します。手動でも作成できますが、GitHub ツールが接続されているため Copilot に依頼しましょう。issue を見つけて PR を作成し、その issue を完了するよう関連付けてからマージします。

> [!NOTE]
> Copilot に GitHub 操作を依頼するのはこの演習が初めてです。プロジェクト テンプレートには GitHub ツールがすでに接続されているため、設定は不要です。Copilot が初めてツールを使うとき、VS Code で GitHub へのサインインを求められる場合があります。表示される手順に従って許可してください。後の演習で Model Context Protocol の仕組みを学びます。

1. codespace で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント ピッカーで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新と新しい発行元ヘルパーの両方を説明する PR を現在のブランチから作成し、関連 issue を解決するよう Copilot に依頼します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を検索して PR を作成します。
5. GitHub 操作を許可するよう求められたら、コマンドを確認して適切に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージし、ローカルのブランチを `main` に戻すよう Copilot に依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. GitHub 操作とシェル コマンドを許可するよう求められたら、内容を確認して適切に許可します。

GitHub Copilot の支援を使ってプルリクエストを作成し、マージできました。

## まとめと次のステップ

指示ファイルから Copilot がコンテキストを取得する方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示でコード生成プロンプトを送信し、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再実行し、提案されたコードが新しい標準に従うことを確認しました。
- 指示の更新とヘルパーの基礎を `main` にコミットし、次の演習で使えるようにしました。

次は[エージェント モード][next-lesson]を使って、Copilot にコードベース全体で新機能を追加してもらいます。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
