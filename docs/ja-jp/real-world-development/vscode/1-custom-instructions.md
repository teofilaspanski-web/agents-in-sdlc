---
title: "演習 1 - カスタム指示 (VS Code)"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

[← 前のレッスン: 前提条件][previous-lesson] · [次のレッスン: エージェントモード →][next-lesson]

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要がある場合や、Copilot に知っておいてほしい背景情報がある場合は、そのコンテキストを利用できるようにします。Copilot に特定のコンテキストを伝える方法はいくつかあります。重要な方法の1つは[指示ファイル][instruction-files]です。指示ファイルは、コード生成に関するガイダンスを提供します。

この演習では、次のことを行います。

- リポジトリのカスタム指示とパスで適用範囲を指定した指示ファイルを通じて、プロジェクト固有のコンテキスト、コーディングガイドライン、ドキュメント標準を Copilot に伝える方法を確認します。
- 現在の指示に沿って、絞り込み機能の最初のデータスライス (パブリッシャーヘルパー) を生成します。
- リポジトリ全体に適用する標準を `.github/copilot-instructions.md` に追加します。
- 同じプロンプトを再送信し、生成コードが新しい標準に従うことを確認します。
- 次の演習で更新した指示を使えるよう、指示の更新と絞り込みの一部を `main` にコミットします。

> [!CAUTION]
> 生成コードは、設定した標準から逸脱する場合があります。Copilot の動作は確定的ではありません。この演習では、指示を更新した後の動作の「傾向」を確認します。出力が文字単位で同じになることを期待しないでください。

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
> 統合開発環境 (IDE) では、指示ファイルは Copilot Chat のコード生成にのみ使われます。コード補完や次の編集候補には使われません。
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

## 変更用ブランチを作成する

ここでは、ベストプラクティスに従って変更用ブランチを作成します。

1. 前の演習で使った codespace に戻ります。
2. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
3. 次のコマンドで新しいブランチを作成して切り替えます。

   ```bash
   git checkout -b custom-instructions
   ```

## 指示を更新する前に Copilot Chat を使う

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

カスタム指示の効果を確認するため、まず現在の指示ファイルを使ってプロンプトを送信します。後で指示ファイルを更新し、同じプロンプトを再送信して違いを確認します。

1. Copilot が必要なコンテキストだけを使えるよう、前の演習で開いたエディタータブを閉じます。
2. `src/lib/publishers.ts` を開き、ヘルパーを置く場所を Copilot に示します。
3. Chat ビューのエージェントドロップダウンから **Agent** を選択し、Copilot がファイルを変更できるようにします。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. 次のプロンプトを送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

5. Copilot がプロジェクトを調べてコードを更新します。多くの場合、ヘルパーファイルとテストが変更されます。
6. 提案されたヘルパーが、最初の引数として `db` クライアントを受け取る型付き関数であり、パブリッシャーの型付き配列を返すことを確認します。これは `src/lib/*.ts` に適用される `.github/instructions/drizzle.instructions.md` のデータレイヤー規約によるものです。
7. 生成されたコードに TSDoc のドキュメントコメントとファイルレベルのコメントヘッダーが**ない**ことを確認します。

> [!CAUTION]
> Copilot は確率的に動作するため、指示がなくてもドキュメントコメントを追加する可能性があります。その場合も問題ありません。ここでは、指示を更新した後の一貫性の向上を確認します。

## リポジトリの標準を追加する

前述のとおり、`.github/copilot-instructions.md` は Copilot にプロジェクト全体の情報を提供するファイルです。コードの提案を改善するため、リポジトリのコーディング標準を追加します。

1. `.github/copilot-instructions.md` を再度開きます。
2. 27行目付近にある **Code formatting requirements** セクションを探します。プロジェクトのコーディング標準は記載されていますが、コード内のドキュメントに関するルールはありません。そのため、生成されたヘルパーにドキュメントコメントがありませんでした。
3. 既存の標準の直後に、次の Markdown を追加します。ファイルヘッダーと TSDoc のドキュメントコメントを追加するよう Copilot に指示します。

   ```markdown
   - Every exported function should have a TSDoc comment describing its purpose, parameters, and return value.
   - Before imports or any code, add a comment block to the file that explains its purpose.
   ```

4. `copilot-instructions.md` を保存します。

> [!TIP]
> 前のレッスンで確認したように、指示ファイルはリポジトリレベル (`.github/copilot-instructions.md`) に作成して全体に適用することも、特定の言語、ファイル種別、タスクに適用する `*.instructions.md` ファイルとして作成することもできます。今回追加したドキュメントコメントのルールのように、プロジェクト全体に適用する標準はリポジトリレベルのファイルに記載します。

## プロンプトを再送信して変更を確認する

1. Copilot Chat に戻り、**New Chat** を選択してコンテキストをクリアします。
2. `src/lib/publishers.ts` を選択し、Copilot に適切なファイルを認識させます。
3. 前と**同じプロンプト**を送信します。

   ```plaintext
   Create or update src/lib/publishers.ts with a data-access helper that returns a list of all publishers with the name and id for each. Apply the file changes.
   ```

4. 提案されたファイルの先頭に、次のようなコメントブロックが追加されていることを確認します。

   ```typescript
   /**
    * Publisher data-access helpers for the Tailspin Toys Crowd Funding platform.
    * Provides functions to retrieve publisher information from the database.
    */
   ```

5. 提案された関数に、次のような TSDoc コメントが含まれていることを確認します。

   ```typescript
   /**
    * Returns a list of all publishers with their id and name.
    *
    * @param db - The Drizzle database client.
    * @returns A promise that resolves to an array of publisher objects.
    */
   ```

新しいプロジェクト標準に従うよう Copilot を導き、次の演習で機能の土台として使うコードに適用しました。

## 指示とブランチをコミットしてプッシュする

指示ファイルも他のリポジトリアセットと同じように、バージョン管理の対象です。指示の変更をコミットして、ブランチをリポジトリにプッシュします。

1. <kbd>Control</kbd>+<kbd>`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>`</kbd> (Windows/Linux) を押して、新しいターミナルを開きます。
2. ターミナルで次のコマンドを実行し、指示とヘルパーの変更が存在することを確認します。

   ```bash
   git status
   ```

3. ターミナルで指示の変更と、ヘルパーの土台を作るために Copilot が変更したすべてのファイルをステージしてコミットします。

   ```bash
   git add .github/copilot-instructions.md src/lib/publishers.ts
   # git status にテストなどの追加変更が表示された場合は、それらのファイルも追加します。
   git commit -m "Add doc comment standards and publishers helper foundation"
   ```

4. ターミナルからブランチをリポジトリにプッシュします。

   ```bash
   git push -u origin custom-instructions
   ```

## プルリクエストを作成してマージする

ブランチをプッシュしたら、バックログのドキュメント標準に関する issue と関連付けてプルリクエストを作成します。手動で作成することもできますが、プロジェクトに接続済みの GitHub ツールを使って Copilot に依頼します。issue を見つけ、PR で issue をクローズするよう指定し、マージまで依頼します。

> [!NOTE]
> この演習では、Copilot に初めて GitHub 上の操作を実行させます。プロジェクトテンプレートでは GitHub のツールがあらかじめワークスペースに接続されているため、セットアップは不要です。Copilot が初めてツールを使うときに、VS Code から GitHub へのサインインを求められる場合があります。画面の指示に従って許可します。後の演習では、この接続の仕組みである Model Context Protocol (MCP) を学びます。

1. codespace 内で Copilot Chat を開きます。
2. <kbd>Control</kbd>+<kbd>Command</kbd>+<kbd>I</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>I</kbd> (Windows/Linux) を押して Copilot Chat ビューを開き、エージェント選択メニューで **Agent** が選択されていることを確認します。
3. 指示ファイルの更新に関する issue を見つけ、現在のブランチから PR を作成するよう、次のプロンプトを Copilot に送信します。PR では issue をクローズすることを明記し、今後の絞り込み作業に備えてパブリッシャーヘルパーの土台も追加したことを記載します。

   ```
   Find the issue related to updating the instructions file. Create a new PR from the current branch, highlight that the PR closes that issue, and include that we also added the publishers helper foundation for the upcoming filtering work.
   ```

4. Copilot が issue を探して PR を作成します。
5. Copilot が GitHub 操作の許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。
6. PR が作成されたら、次のプロンプトでマージしてローカルのブランチを `main` に戻すよう依頼します。

   ```
   Merge the PR into main. Then return to main locally, and pull the latest code so we are up to date.
   ```

7. Copilot が GitHub 操作やシェルコマンドの実行許可を求めたら、コマンドを確認して、適切な場合に **Allow** を選択します。

GitHub Copilot の支援でプルリクエストを作成し、マージできました。

## まとめと次のステップ

このプロジェクトで Copilot が指示ファイルからコンテキストを読み取る方法を確認し、VS Code の Copilot Chat を使って次のことを行いました。

- 既存の指示ファイルを使ってコード生成プロンプトを送り、Copilot の出力を確認しました。
- `.github/copilot-instructions.md` にリポジトリ全体に適用する標準を追加しました。
- 同じプロンプトを再送信し、生成されたコードが新しい標準に従うことを確認しました。
- 指示の更新とヘルパーの土台をコミットして `main` に反映し、次の演習で使えるようにしました。

次は[エージェントモード][next-lesson]で指示を活用し、Copilot にコードベース全体にわたる新機能を追加させます。

## 参考資料

- [GitHub Copilot のカスタマイズに使う指示ファイル][instruction-files]
- [カスタム指示のベストプラクティス][instructions-best-practices]
- [Copilot のカスタム指示を改善する5つのヒント][copilot-instructions-five-tips]
- [GitHub Copilot の個人用カスタム指示][personal-instructions]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[previous-lesson]: ../0-prerequisites/
[next-lesson]: ../2-agent-mode/
[instruction-files]: https://code.visualstudio.com/docs/copilot/copilot-customization
[instructions-best-practices]: https://docs.github.com/enterprise-cloud@latest/copilot/using-github-copilot/coding-agent/best-practices-for-using-copilot-to-work-on-tasks#adding-custom-instructions-to-your-repository
[personal-instructions]: https://docs.github.com/copilot/customizing-copilot/adding-personal-custom-instructions-for-github-copilot
[copilot-instructions-five-tips]: https://github.blog/ai-and-ml/github-copilot/5-tips-for-writing-better-custom-instructions-for-copilot/
[awesome-copilot]: https://github.com/github/awesome-copilot
