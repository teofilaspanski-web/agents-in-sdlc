---
title: "レッスン 4 - カスタム指示で Copilot を導く"
description: "リポジトリの指示を確認し、ドキュメント標準を追加して、絞り込み機能のコードに適用します。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

生成 AI を使うときは、コンテキストが重要です。特定の方法でタスクを実行する必要があるなら、その指針を Copilot が利用できるようにします。[指示ファイル][instruction-files]には、求めるコードの内容だけでなく、コードの構成方法も記述できます。絞り込み機能を実装したので、Copilot が参照した指示を確認し、ドキュメント標準を追加して、コードに適用します。

このレッスンでは、次のことを行います。

- リポジトリの指示とパスで適用範囲を指定した指示ファイルがエージェントに読み込まれる方法を確認します。
- コーディング標準に従うよう、指示ファイルを更新します。
- 指示ファイルがコードに与える影響を確認します。

## シナリオ

Tailspin Toys には、開発手法に関するガイドラインと要件があります。具体的には、次のとおりです。

- コメントにはコードを言い換えるのではなく、意図や自明ではない判断を説明します。
- `db/` と `src/lib/` のエクスポートされた関数では、目的、パラメーター、戻り値を TypeScript Documentation (TSDoc) / JavaScript Documentation (JSDoc) で説明します。該当する場合は、注入可能な `db` 引数も含めます。
- 再利用可能な Astro コンポーネントでは `Props` の契約を文書化し、関連コードが変更された際にコメントも最新の状態にします。
- 既存の書式設定と lint の指針を維持します。

指示ファイルを使って、Copilot がこれらのプラクティスに沿ってタスクを実行するために必要な情報を提供します。

## 指示ファイル

カスタム指示を使うと、コーディングスタイルや要件に関するコンテキストや希望を Copilot に伝えられます。これにより、より適切な提案やコードスニペットを得られます。リポジトリ全体に適用する指示も、特定のファイル種別に適用する指示も作成できます。

指示ファイルには次の2種類があります。

- `.github/copilot-instructions.md` は、リポジトリに対する**すべての**リクエストで Copilot に送信される単一の指示ファイルです。大半のリクエストに関係するプロジェクト全体の情報を記載します。
- `.github/instructions/*.instructions.md` は、特定の言語、ファイル種別、タスクに関するガイドラインを提供します。

> [!NOTE]
> その他の指示形式やサポート状況は、環境によって異なります。特定の形式に依存する前に、[カスタム指示のサポートに関するリファレンス][custom-instructions-support]を確認してください。

## プロジェクトのカスタム指示ファイルを確認する

スタータープロジェクトには、すでに指示ファイルが用意されています。変更を加える前に、現在の指示とその影響を確認しましょう。

1. Codespace に戻ります。
2. Codespaces エディターで (ターミナルではなく) `.github/copilot-instructions.md` を開きます。
3. プロジェクトの簡単な説明とコーディング指針を確認します。この指示は、リポジトリ内で Copilot と行うすべてのやりとりに適用されます。
4. `.github/instructions` フォルダーを開いて内容を確認します。Astro ファイル、Drizzle データレイヤー、テストなどの指示があります。
5. `.github/instructions/unit-tests.instructions.md` を開きます。先頭の `applyTo` フィールドが、適用対象のファイルを決める glob を指定しています。
6. `.github/instructions/drizzle.instructions.md` を開き、他の指示ファイルやプロジェクト内の既存ファイルへのリンクを確認します。指示を小さく再利用可能なファイルに分け、Copilot が参考にできる例を示せます。

## チームの指針に合わせて指示ファイルを更新する

既存のファイルは良い出発点ですが、追加すべき指針があります。生成された TypeScript に TSDoc コメントを追加するよう、中心となる `copilot-instructions.md` を変更します。

1. `.github/copilot-instructions.md` で、35行目付近にある **Code formatting guidance** セクションを探します。
2. 最後の箇条書きとして、次の内容を追加します。

   ```markdown
   - All new TypeScript should contain TSDocs comments for documentation purposes.
   ```

ファイルは自動的に保存されます。

## 更新した指針を使う

コマンドラインインターフェイス (CLI) 版の Copilot である Copilot CLI は、会話の開始時にリポジトリの指示を読み込みます。指示を保存した後、絞り込み機能のコンテキストを保ったまま会話を終了して再開し、新しい指示を読み込ませます。

1. 絞り込みの会話がまだ開いている場合は、`/exit` を入力して終了します。Copilot CLI は会話を自動的に保存します。
2. リポジトリのルートにある `game-filters-cli` ブランチから、保存済みの会話を再開します。

   ```bash
   copilot --resume
   ```

   選択を求められた場合は絞り込みの会話を選び、`game-filters-cli` ブランチがチェックアウトされていることを確認します。
3. Copilot に次のプロンプトを送信します。

   ```plaintext
   We just updated our instructions and code guidance. Can you please update the code you generated to match that guidance?
   ```

4. `/diff` を入力し、変更された TypeScript ファイルを確認します。新しい TSDoc コメントが追加され、コードを正確に説明していることを確認します。

## まとめと次のステップ

Copilot CLI が指示ファイルからコンテキストを読み取る方法を確認し、新しい標準を機能に適用しました。具体的には、次のことを行いました。

- リポジトリの指示とパスで適用範囲を指定した指示ファイルがエージェントに読み込まれる方法を確認しました。
- 指示ファイルを更新して新しい標準を追加しました。
- 指示ファイルがコードに与える影響を確認しました。

次は、[再利用可能な quality-checks スキルをカスタマイズして実行し][next-lesson]、lint とテストを一貫して実行できるようにします。

## 参考資料

- [Copilot CLI のカスタム指示を追加する][instruction-files]
- [カスタム指示のサポート][custom-instructions-support]
- [効果的なカスタム指示を作成するためのベストプラクティス][instructions-best-practices]
- [Awesome Copilot — 指示ファイルなどのリソース集][awesome-copilot]

[instruction-files]: https://docs.github.com/copilot/how-tos/copilot-cli/customize-copilot/add-custom-instructions
[instructions-best-practices]: https://docs.github.com/copilot/concepts/prompting/response-customization#writing-effective-custom-instructions
[awesome-copilot]: https://awesome-copilot.github.com/
[custom-instructions-support]: https://docs.github.com/copilot/reference/custom-instructions-support
[previous-lesson]: ../3-agent-modes/
[next-lesson]: ../5-agent-skills/
