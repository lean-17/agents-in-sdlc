---
title: "演習 1 - カスタム指示 (Cloud agent)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot cloud agent →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを参照できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に何をしてほしいかだけでなく、どのように実行してほしいかも伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスを対象とする指示ファイルを使って、プロジェクト固有のコンテキスト、コーディング ガイドライン、ドキュメント標準が Copilot に伝わる方法を確認します。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加します。

> [!NOTE]
> VS Code と CLI のハーネスとは異なり、ここではプロンプトの「実行前と実行後」を比較しません。Copilot cloud agent は GitHub issue に対して非同期で動作するため、リアルタイムで横並びに効果を示すのは困難です。後の演習で cloud agent が作成したプルリクエストをレビューするときに、指示ファイルの影響を確認します。

## 指示ファイル

### シナリオ

Tailspin Toys のような開発会社には、開発方法に関するガイドラインや要件があります。たとえば、次のようなものです。

- データ層には常にユニット テストが必要です。
- UI はダーク モードで、現代的な見た目にします。
- TSDoc のドキュメント コメントを使ってコードを文書化します。
- 各ファイルの先頭に、そのファイルの役割を説明するコメント ブロックを追加します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行するために必要な情報を与えます。

### カスタム指示

カスタム指示を使うと、コーディング スタイルや要件に関するコンテキストや希望を Copilot に伝えられます。好みのコーディング規約、ライブラリ、コメントの形式などを指定できるため、より適切な提案やコード スニペットを得るのに役立ちます。リポジトリ全体に適用する指示と、特定のファイルやタスクに適用する指示を作成できます。

指示ファイルには次の 2 種類があります。

- `.github/copilot-instructions.md` は、リポジトリへの**すべての**リクエストで Copilot に送信される単一の指示ファイルです。チャットや CLI で Copilot に送るリクエストの多くに関係する、プロジェクト全体のコンテキストを記述します。技術スタック、開発対象の概要、ベスト プラクティス、全般的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` ファイルは、特定のタスクやファイルの種類を対象に作成できます。TypeScript や Astro などの言語、UI コンポーネントやユニット テストの作成などのタスクに対する指示を記述できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat でのコード生成にのみ使われ、コード補完や次の編集の提案には使われません。
>
> Copilot Chat、Copilot CLI、Copilot cloud agent は、リポジトリ レベルの指示ファイルと `applyTo` フロントマターを持つ `*.instructions.md` ファイルの両方を使用します。
>
> また、Copilot は AGENTS.md や CLAUDE.md など、[ほかの標準の指示ファイル][custom-instructions-support]にも対応しています。

### 指示ファイルを管理するためのベスト プラクティス

指示ファイルの作成について詳しく説明することはこの演習の範囲外ですが、サンプル プロジェクトには代表的なアプローチが含まれています。要点は次のとおりです。

- `copilot-instructions.md` には、開発対象やプロジェクトの構成、全般的なコーディング標準など、プロジェクト全体に関するガイダンスを記述します。
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

このリポジトリに含まれる指示ファイルを読んでみましょう。コアとなる `copilot-instructions.md` と、さまざまなタスク向けの `*.instructions.md` ファイルがあります。エディターまたは GitHub の Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. プロジェクトの簡単な説明と、**Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下にある **GitHub Actions Workflows** のガイダンスも確認してください。これらは Copilot とのやり取り全般に適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データ層、テストなどの指示ファイルがあります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドは、指示の適用対象ファイルを決める glob です。たとえば `**/*.test.ts` に一致する TypeScript テスト ファイルが対象になります。
5. このプロジェクトのユニット テスト作成に固有の指示を確認します。
6. 最後に `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。`unit-tests.instructions.md` などの指示ファイルや、プロジェクト内の既存ファイルへのリンクがあります。これにより、大きな指示を再利用可能な小さなファイルに分けたり、Copilot に参考例を示したりできます (ここでのパスはリポジトリ ルートではなく、指示ファイルからの相対パスです)。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントはまだ必須ではありません。次の手順で、TSDoc のドキュメント コメントとファイル コメント ヘッダーのルールを追加します。

## リポジトリ標準を追加する

次に編集するのは、ドキュメントを TSDoc のコメントとしてコード内に記述し、ファイル レベルのコメント ヘッダーも追加するというプロジェクト全体のルールです。後の演習で issue を割り当てると、cloud agent がこの指示を参照します。

編集する前に、作業用のブランチを作成します (cloud agent は issue の対象ブランチから分岐しますが、ここでは編集内容を明確にコミットします)。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前に説明したように、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するために、リポジトリのコーディング標準を記録しましょう。

1. `.github/copilot-instructions.md` を開き直します。
2. 27 行目付近にある **Code formatting requirements** セクションを見つけます。コーディング標準が記載されていますが、コード内ドキュメントのルールはありません。そのため、生成されたヘルパーにドキュメント コメントがありませんでした。
3. 次の Markdown を既存の標準の直後に追加して、ファイル コメント ヘッダーと TSDoc のドキュメント コメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、リポジトリ レベルの `.github/copilot-instructions.md` には全般的な指示を、`*.instructions.md` ファイルには言語、ファイルの種類、タスク固有の指示を記述できます。ドキュメント コメントのルールのようなプロジェクト全体の標準には、リポジトリ レベルのファイルが適しています。

## 指示をコミット、プッシュしてマージする

cloud agent は、issue の対象ブランチにある指示ファイルを読み込みます。次の演習で Copilot に issue を割り当てるときは `main` からブランチが作成されるため、cloud agent が指示を使えるよう、変更を `main` に反映する必要があります。

1. ステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを作成してマージします。次の演習で issue に割り当てたとき、cloud agent はこの指示を読み込みます。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せずに、変更を `main` にコミットできます。ここでは、実際のプロジェクトでの進め方を再現するためにブランチを使っています。

## まとめと次のステップ

このプロジェクトの指示ファイルから Copilot がコンテキストを取得する方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスの後半で、cloud agent が作成するプルリクエストをレビューするときに、この標準が適用されていることを確認します。

次に、[最初の issue を Copilot cloud agent に割り当てます][next-lesson]。

## リソース

- [GitHub Copilot のカスタマイズ用指示ファイル][instruction-files]
- [カスタム指示を作成するためのベスト プラクティス][instructions-best-practices]
- [Copilot 向けのカスタム指示を改善する 5 つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
