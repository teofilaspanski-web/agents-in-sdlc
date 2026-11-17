---
title: "演習 1 - カスタム指示 (クラウドエージェント)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: Copilot クラウドエージェント →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。[指示ファイル][instruction-files]を使うと、Copilot に実行してほしいことだけでなく、その方法も伝えられます。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスで適用範囲を指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。

> [!NOTE]
> VS Code や CLI のハーネスとは異なり、この演習では指示の更新前と更新後に同じプロンプトを実行しません。Copilot クラウドエージェントは GitHub issue を使って非同期で作業するため、リアルタイムで並べて効果を示すのが難しいためです。このハーネスの後半で、クラウドエージェントが作成したプルリクエストを確認するときに、指示ファイルの効果を確認できます。

## 指示ファイル

### シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- データレイヤーには必ず単体テストを用意します。
- UI はダークモードにし、現代的な見た目にします。
- コードに TypeScript Documentation (TSDoc) のドキュメントコメントを追加します。
- 各ファイルの先頭に、ファイルの役割を説明するコメントブロックを追加します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行するために必要な情報を提供します。

### カスタム指示

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや希望を Copilot に伝えられます。これにより、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。Copilot Chat や CLI へのほとんどのリクエストに関係する、プロジェクト全体の情報を記載します。技術スタック、開発中の内容の概要、ベストプラクティス、全体的なガイダンスなどが該当します。
- `.github/instructions/*.instructions.md` は、特定のタスクやファイル種別に適用する指示ファイルです。TypeScript や Astro などの言語、UI コンポーネントの作成や単体テストの作成などのタスクに関するガイドラインを記載できます。

> [!NOTE]
> IDE では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
>
> Copilot Chat、Copilot CLI、Copilot クラウドエージェントは、リポジトリレベルの指示と、`applyTo` フロントマターで対象を指定した `*.instructions.md` ファイルの両方を使ってコードを生成します。
>
> Copilot は、AGENTS.md や CLAUDE.md など、別の規約に基づく指示ファイルにも対応しています。[指示ファイルのサポート][custom-instructions-support]を参照してください。

### 指示ファイルを管理するためのベストプラクティス

指示ファイルの作成に関する詳しい説明は、このワークショップの範囲外です。ただし、サンプルプロジェクトに含まれる例から、一般的な方法を確認できます。

- `copilot-instructions.md` には、開発中の内容、プロジェクト構成、全体的なコーディング標準など、プロジェクト全体に関する指針を記載します。
- `*.instructions.md` は、単体テスト、Astro コンポーネント、データレイヤーなどのファイル種別、または特定のタスクに関する指示に使います。
- 自然な言葉で、明確な指針を記載します。コードの良い例と避けるべき例も示します。

指示ファイルの作成方法は1つに限られません。AI の使い方と同様に、プロジェクトで試しながら、最も効果的な方法を見つけてください。

> [!TIP]
> GitHub Copilot を使うすべてのプロジェクトに、充実した指示ファイルを用意することをお勧めします。このプロジェクトのファイルを確認すると、[UI 更新][ui-instructions]や [Astro][astro-instructions]など、さまざまなタスク用の指示があります。
>
> Copilot に指示ファイルを作成させることもできます。利用する環境によって方法は異なります。たとえば、VS Code の **Configure Chat → Generate Agent Instructions** や、Copilot CLI の `/init` を使います。各環境の演習では、該当する方法を説明します。
>
> テンプレートや出発点を探している場合は、指示ファイル、カスタムエージェント、その他のリソースをまとめたリポジトリ [awesome-copilot][awesome-copilot] を確認してください。

[ui-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md
[astro-instructions]: https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/astro.instructions.md
[awesome-copilot]: https://github.com/github/awesome-copilot
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support

## プロジェクトのカスタム指示ファイルを確認する

このリポジトリに含まれる指示ファイルを確認します。中心となる `copilot-instructions.md` と、タスク別の `*.instructions.md` ファイルがあります。エディターまたは GitHub Web UI で開きます。

1. `.github/copilot-instructions.md` を開きます。
2. **Agent notes**、**Code standards**、**Scripts**、**Repository Structure** などのセクションを確認します。**Code standards** の下には **GitHub Actions Workflows** の指針があります。これらの指示は、Copilot とのあらゆるやりとりに適用されます。
3. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
4. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、リポジトリのルートからの相対 glob を指定しています。たとえば、`**/*.test.ts` に一致する TypeScript のテストファイルが対象です。
5. このプロジェクトで単体テストを作成する際の指示を確認します。
6. `.github/instructions/drizzle.instructions.md` を開き、末尾までスクロールします。他の指示ファイル (例: `unit-tests.instructions.md`) やプロジェクト内の既存ファイルへのリンクを確認します。これにより、大きな指示を小さく再利用可能なファイルに分け、Copilot がコードの生成時に参考にできる例を示せます。ここで使うパスは、リポジトリのルートではなく指示ファイルからの相対パスです。

> [!NOTE]
> `copilot-instructions.md` の **Code formatting requirements** セクションにはプロジェクトのコーディング標準が記載されていますが、コード内のドキュメントに関する要件はまだありません。次の手順で、TSDoc のドキュメントコメントとファイル先頭のコメントに関するルールを追加します。

## リポジトリ全体に適用する標準を追加する

この演習で編集するのは、プロジェクト全体に適用するルールの追加だけです。コード内のドキュメントを TSDoc のコメントとファイルレベルのコメントで記載するよう指定します。次の演習でクラウドエージェントに issue を割り当てると、この指示が使われます。

編集の前に、変更用のブランチを準備します。クラウドエージェントは issue が対象とするブランチの指示を読み取りますが、ここでは変更をきちんとコミットします。

1. codespace のターミナルで、新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions-cloud
   ```

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供します。コードの提案を改善するため、リポジトリのコーディング標準を明記しましょう。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。エクスポートされた関数にファイル先頭のコメントと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメントコメントのルールのように、プロジェクト全体に適用する標準はリポジトリレベルのファイルに記載します。

## 指示の変更をコミット、プッシュ、マージする

クラウドエージェントは、issue が対象とするブランチの指示ファイルを読み取ります。次の演習で issue を Copilot に割り当てると、Copilot は `main` からブランチを作成します。クラウドエージェントが指示を読み取れるよう、変更を `main` にマージする必要があります。

1. ファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md
   git commit -m "Add doc comment and file-header standards to copilot instructions"
   ```

2. ブランチをプッシュします。

   ```bash
   git push -u origin custom-instructions-cloud
   ```

3. github.com で `custom-instructions-cloud` から `main` へのプルリクエストを開き、マージします。次の演習で issue に割り当てたタスクに取り組むとき、クラウドエージェントがこの指示を読み取れるようになります。

> [!TIP]
> このワークショップでは `main` に直接作業してもかまいません。その場合はブランチを作成せず、`main` に直接コミットできます。ここでは、実際のプロジェクトでの作業方法を再現するためにブランチを使っています。

## まとめと次のステップ

Copilot がこのプロジェクトの指示ファイルからコンテキストを読み取る方法を確認し、`.github/copilot-instructions.md` にリポジトリ全体の標準を追加しました。このハーネスでクラウドエージェントが作成するプルリクエストを確認するときに、追加した標準が適用されているか確かめられます。

次は、Copilot クラウドエージェントに[最初の issue を割り当てます][next-lesson]。

## 参考資料

- [GitHub Copilot のカスタム指示ファイル][instruction-files]
- [カスタム指示のベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-cloud-agent/
[instruction-files]: https://docs.github.com/copilot/customizing-copilot/about-customizing-github-copilot-chat-responses
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
