---
title: "演習 2 - Copilot エージェントモードで機能を追加する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

最も簡単な更新でも、通常は複数のファイルを変更し、テストを実行する必要があります。開発者は通常、必要なファイルを探して変更し、テストを実行して、問題をデバッグします。抜けていたファイルがわかると、さらに変更を加えます。この繰り返しが続くこともあります。

ここで役立つのが Copilot エージェントモードです。

Copilot エージェントモードは、IDE 内でより自律的に作業するように設計されています。開発者と同様に、既存のプロジェクト構造を調べ、必要な変更を行い、テストなどのタスクを実行し、見つかった問題を自動的に修正します。エージェントモードを使ってサイトに機能を追加する方法を確認しましょう。

> [!NOTE]
> 名前は似ていますが、エージェントモードとクラウドエージェントは異なる用途の機能です。エージェントモードは IDE 内でタスクを実行するため、すばやいフィードバックとやりとりができます。クラウドエージェントはチームの一員のように非同期で作業し、issue とプルリクエストを通じて開発者とやりとりします。

この演習では、次のことを学びます。

- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを見つけ、複数ファイルにわたって変更する方法。
- GitHub Copilot エージェントモードを使って、UI とデータレイヤーにわたる新機能を実装する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストを確認する方法。

## シナリオ

ゲームの一覧が増えてきたため、ユーザーがカテゴリやパブリッシャーで絞り込めるようにします。前の演習ではパブリッシャー用のヘルパーを追加しました。次は Copilot エージェントモードを使い、残りのデータレイヤー、UI、テストを完成させます。

## Tailspin Toys の Web サイトを実行する

変更を加える前に、Tailspin Toys の現在の機能を確認します。

この Web サイトは、開発者向けのテーマを持つボードゲームのクラウドファンディングプラットフォームです。ゲームの一覧と詳細を表示できます。このサイトは単一の Astro アプリで、ビルド時にページを静的な HyperText Markup Language (HTML) としてレンダリングします。ページは Drizzle のオブジェクトリレーショナルマッパー (ORM) を使って、フロントマターからローカルの SQLite データベースを直接検索します。別個のバックエンドのアプリケーションプログラミングインターフェイス (API) やクライアント側 UI フレームワークはありません。再利用可能なデータアクセスヘルパーは `src/lib/` にあり、インタラクティブ機能には標準のドキュメントオブジェクトモデル (DOM) API を使う小さな Astro `<script>` を使用します。

### Web サイトを起動する

`npm run dev` タスクを使うと、単一の Astro 開発サーバーを起動できます。次の手順で GitHub Codespace 上で実行します。

1. Codespace に戻ります。前の演習から `main` に戻っているはずです。
2. Codespace 内で新しいターミナルを開きます。<kbd>Control</kbd>+<kbd>\`</kbd> (Mac) または <kbd>Ctrl</kbd>+<kbd>\`</kbd> (Windows/Linux) を押します。
3. 絞り込み作業用のブランチを作成して切り替えます。

   ```bash
   git checkout main
   git pull
   git checkout -b filtering-vscode
   ```

4. 次のコマンドを実行して Web サイトを起動します。

   ```bash
   npm run dev
   ```

   Astro 開発サーバーの準備ができると、次のような URL を示すメッセージが表示されます。

   ```bash
   🚀 Tailspin Toys is ready!
      Astro server: http://localhost:4321
      Press Ctrl-C to stop.
   ```

> [!NOTE]
> `http://localhost:4321` を開くように求めるダイアログが表示された場合は、**x** を選択して閉じます。

5. ターミナルに表示された Astro サーバーのアドレス `http://localhost:4321` を、<kbd>Command</kbd> (Mac) または <kbd>Ctrl</kbd> (Windows/Linux) を押しながら選択して、Web サイトを開きます。

> [!NOTE]
> Codespace のターミナルで localhost の URL を選択すると、自動的に `https://<your-codespace-name>-4321.app.github.dev/` に転送されます。これは、Web サーバーを実行している codespace へのプライベートトンネルです。

### Web サイトを確認する

Web サイトが起動したら、機能を確認できます。主な機能は次のとおりです。

- **ホームページ**: タイトル、画像、説明とともにボードゲームを一覧表示します。
- **ゲーム詳細ページ**: ゲームを選択すると、タイトル、説明、パブリッシャー、カテゴリなどの情報を含む詳細ページが表示されます。

## Copilot でバックログを確認する

> [!TIP]
> **Copilot Chat を開く**
>
> 以下の演習を始める前に、codespace に戻り、Copilot Chat パネルを開き、**New Chat** を選択して新しい会話を始めます。モードとモデルの選択は演習によって異なります。必要な場合は、各手順で説明します。

初期状態の Web サイトは動作しますが、新しい機能を追加して改善しましょう。まずバックログを確認します。テンプレートからプロジェクトをセットアップしたときに issue のバックログが自動作成されています。GitHub Copilot に内容を表示させます。

1. Chat ビューのエージェントドロップダウンから **Agent** を選択します。**Agent** エージェントは、自律的に変更の計画と実装を行い、ターミナルコマンドを実行してツールを呼び出します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

2. 利用可能なモデルの一覧から **Claude Sonnet 4.5** を選択します。

> [!CAUTION]
> ワークショップの作成者が特定のモデルを推奨しているわけではありません。このワークショップでは Claude Sonnet 4.5 を使用したため、手順にも記載しています。提案されるコードに一貫性を持たせ、スムーズに進められることを期待しています。ただし、大規模言語モデル (LLM) は確率的に動作するため、記載内容と異なる提案が表示されることがあります。これは通常の動作です。

> [!NOTE]
> 確率的な動作のため、Copilot が別の MCP コマンドを使うことがありますが、タスクは完了できるはずです。

3. 次のプロンプトを Copilot に送信して、issue のバックログについて質問します。

   ```plaintext
   Please show me the backlog of items from my GitHub repository. Help me prioritize them based on those which will be most useful to the user.
   ```

4. すべての issue を一覧表示するコマンドを実行するには、**Continue** を選択します。
5. 生成された issue の一覧を確認します。

Copilot がユーザーに最も役立つと思われる項目を優先順位付けしたことを確認します。

## 指示ファイルを確認する

エージェントにコードを生成させる前に、作業のコンテキストを Copilot に伝える指示ファイルを確認します。Web サイトに機能を追加する方法に関する情報が含まれている [ユーザーインターフェイス (UI) 指示ファイル](https://github.com/github-samples/tailspin-toys/blob/main/.github/instructions/ui.instructions.md)を使います。

1. codespace で `.github/instructions/ui.instructions.md` に移動します。
2. 機能追加の進め方に関する全体的な指針を確認します。次の内容が含まれます。
   - アーキテクチャの概要。
   - コンポーネントの設計、テスト容易性、アクセシビリティの原則。
   - Astro、Tailwind CSS、Drizzle など、ファイル種別に応じた指示へのリンク。

> [!TIP]
> 指示ファイルでは、他の指示ファイルやプロジェクト内のファイルを参照できます。パスは指示ファイルの場所からの相対パスです。指示を再利用し、大きな指示を小さく管理しやすい単位に分け、Copilot がコードの生成時に参考にできる例やテンプレートを示せます。

## 絞り込み機能を実装する

絞り込みを完成させるには、少なくとも次の3つの変更が必要です。

- データレイヤー (`src/lib/`) に残りの絞り込みロジックを追加または調整します。
- 絞り込みの動作を確認するテストを追加または更新します。
- ゲーム一覧ページに絞り込み UI を追加します。

さらに、コードベースにマージする前にテストを実行して成功させる必要があります。Copilot エージェントモードにこれらのタスクを実行させましょう。

1. 現在の会話を続けるか、**New Chat** を選択して新しい会話を始めます。
2. **Add Context**、**Instructions**、**ui** の順に選択し、指示ファイルを追加します。

   ![UI 指示ファイルを選択する例](../../../_images/ex3-select-instructions-file.png)

3. Chat ビューで **Agent** が選択されていることを確認します。

   ![Chat ビューのエージェント選択メニュー](../../../_images/shared-chat-mode-selector.png)

4. モデルとして **Claude Sonnet 4.5** が選択されていることを確認します。
5. バックログ内の関連 issue に基づいて機能を実装するよう、次のプロンプトを Copilot に送信します。

   ```plaintext
   Please update the site to include filtering by publisher and category based on the requirements from the related GitHub issue in the backlog. A publishers helper already exists from the previous exercise, so preserve and refine that work as needed while completing the remaining data-layer, UI, and tests. Ensure all tests are passing before completion. The server is already running, so you do not need to start it up.
   ```

6. Copilot がプロジェクトを調べ、機能に関連するファイルを見つける様子を確認します。データレイヤーのヘルパー、UI、テストを探した後、ファイルを変更してテストを実行します。

   ![Copilot がプロジェクトを調べている画面](../../../_images/ex3-agent-mode-explores.png)

> [!NOTE]
> Copilot はプロジェクトの調査、ファイルの変更、テストの実行などを行うため、タスクの複雑さやコードベースによっては数分かかります。作業中、コードエディターに **Keep** と **Undo** が表示されることがあります。作業が完了すると、すべての変更に **Keep** または **Undo** が表示されます。作業中に選択する必要はありません。

7. Copilot に求められたら、**Continue** を選択してテストを実行します。

   ![Copilot Chat でテストの実行を確認するダイアログ](../../../_images/ex3-agent-mode-run-tests.png)

8. 作業中に一時停止したり、テストが失敗したりする場合があります。問題ありません。Copilot はコード生成とテストを繰り返し、エラーを検出しなくなるまで作業します。

   ![Copilot エージェントモードの完了したチャットセッション](../../../_images/ex3-agent-mode-proposed-changes.png)

9. 生成されたコードに問題がないか確認します。

> [!CAUTION]
> Copilot などの AI ツールが生成したコードは、必ず確認してください。

10. Web サイトを実行しているブラウザーに戻り、新機能を試します。
11. 正常に動作することを確認し、コードをレビューしたら、Copilot Chat ウィンドウで **Keep** を選択します。

## まとめと次のステップ

おめでとうございます。この演習では、GitHub Copilot エージェントモードを使って Tailspin Toys の Web サイトに機能を追加しました。次のことを学びました。

- GitHub Copilot エージェントモードを使って、UI とデータレイヤーにわたる新機能を実装する方法。
- Copilot エージェントモードがプロジェクトを調べ、関連ファイルを見つけ、複数ファイルにわたって変更する方法。
- コードベースにマージする前に、Copilot エージェントモードが生成した変更とテストを確認する方法。

次は、[Playwright MCP サーバーで機能をテストし][next-lesson]、プルリクエストを開きます。

### 追加の演習 - ページングを実装する

ゲームの一覧が増えると、ページングが必要になります。この演習で学んだことを使い、Copilot にサイトのページングを実装させてみましょう。次の点を検討してください。

- 既存の指示ファイルを使うなど、既存のベストプラクティスに従います。
- ページサイズをユーザーが選べるようにするか、固定するかなど、ページングの実装方法を検討します。
- 希望どおりの実装になるよう、プロンプトに必要な情報を含めます。
- Copilot に変更を依頼してコンテキストを追加しながら、何度か反復する必要がある場合があります。Copilot を使うときの通常の進め方です。

## 参考資料

- [Copilot の Ask、Edit、Agent モード: 機能と使い分け][choose-mode]
- [VS Code のエージェントモード][vs-code-agent-mode]

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-mcp/
[choose-mode]: https://github.blog/ai-and-ml/github-copilot/copilot-ask-edit-and-agent-modes-what-they-do-and-when-to-use-them/
[vs-code-agent-mode]: https://code.visualstudio.com/docs/copilot/chat/chat-agent-mode
