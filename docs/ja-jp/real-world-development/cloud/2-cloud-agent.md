---
title: "演習 2 - GitHub Copilot クラウドエージェント"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: カスタム指示][previous-lesson] |
|:--|

技術的負債に悩んでいない組織は、ほとんどないでしょう。未解決のセキュリティ問題、更新が必要なレガシーコード、実装する時間がなくバックログに残った機能リクエストなどがあります。GitHub Copilot のクラウドエージェントは、コードの更新や機能の追加などを自律的に実行できます。作業が完了すると、人間の開発者がレビューできるドラフトのプルリクエスト (PR) を作成します。面倒な作業を任せて開発プロセスを加速し、開発者がより大きな課題に集中できるようにします。

Copilot クラウドエージェントを使って、次の内容を確認します。

- コード生成環境のカスタマイズ。
- 操作を安全に実行する方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

## シナリオ

Tailspin Toys には対処したい技術的負債があります。最初のサイトを作成した委託業者が、ドキュメントを適切に整備していませんでした。実際、ほとんど何も文書化されていません。まず、アプリケーション内のエクスポートされたすべての関数に TSDoc のドキュメントコメントを追加します。

また、デザインチームはゲームを見つけやすくしたいと考えています。各ゲームの詳細ページに、同じカテゴリに属する他のタイトルを「関連ゲーム」として表示し、ユーザーが閲覧を続けられるようにします。現時点では、受け入れテストでユーザーエクスペリエンス (UX) を試せる程度の実装で十分です。この作業は現在ブロッカーですが、優先度の高い別の issue があります。

どちらも、優先順位が下がりやすいタスクです。Copilot クラウドエージェントに割り当てるのに適しています。Copilot クラウドエージェントが非同期で作業している間、開発者は別のタスクに取り組み、後で戻って作業をレビューし、期待どおりの結果になっているか確認できます。

## GitHub Copilot クラウドエージェントについて

[GitHub Copilot クラウドエージェント](https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent#overview-of-copilot-cloud-agent-formerly-copilot-coding-agent)は、人間の開発者と同じようにバックグラウンドでタスクを実行できます。[GitHub issue を Copilot に割り当てる][start-copilot-sessions]など、複数の方法で作業を依頼できます。割り当てると、Copilot は進行状況を追跡するドラフトのプルリクエストを作成し、環境をセットアップしてタスクに取りかかります。セッションの進行中でも完了後でも、その内容を確認できます。提案された解決策のレビュー準備ができると、プルリクエストで開発者に通知します。

## 範囲を適切に定めた指示の重要性

GitHub Copilot に魔法のような機能はありません。考える作業を省略できるわけではありません。一見簡単そうな作業でも、詳しく調べると複雑さが明らかになります。[Copilot クラウドエージェントに依頼するタスクの範囲][cloud-agent-best-practices]を慎重に決めてください。AI ペアプログラマーとして扱い、段階的に進めながら、学習、試行、調整を続けます。生成 AI を導入しても、ソフトウェア開発の基本は変わりません。

## このリポジトリのカスタム指示

前の演習では、カスタム指示と Copilot の作業に対する効果を確認しました。カスタム指示を実際に試したことがあれば、`.github/copilot-instructions.md` の動作を確認したはずです。まだの場合は、クラウドエージェントに作業を割り当てる前に、このリポジトリに用意された指示ファイルを確認しましょう。後でその影響を確認できます。

リポジトリの GitHub Web UI で次のファイルを開きます。codespace を起動済みの場合は、そちらで開いてもかまいません。

- `.github/copilot-instructions.md` — **Code standards** セクション、特に TypeScript の規約と TSDoc のドキュメントコメントに関する要件を確認します。
- `.github/instructions/unit-tests.instructions.md` — `applyTo` フロントマターが `**/*.test.ts` ファイルに指示を適用していることを確認します。

次のセクションで `Code lacks documentation` issue をクラウドエージェントに割り当てると、生成されたプルリクエストに TSDoc のドキュメントコメント、TypeScript の規約、ファイルヘッダーコメントが反映されます。後の演習で PR をレビューするときに、改めて確認します。

## Copilot クラウドエージェントの開発環境をセットアップする

誰がコードを書く場合でも、通常は適切な状態を保つために特定の環境とセットアップスクリプトが必要です。ソフトウェアエンジニアのように作業する Copilot にタスクを割り当てるときも同様です。

クラウドエージェントは作業環境に [GitHub Actions][github-actions] を使います。`.github/workflows/copilot-setup-steps.yml` に設定する[専用のセットアップワークフロー][setup-workflow]を作成すると、作業を始める前に必要な開発ツールや依存関係を利用できるようになります。このラボの手順を円滑に進められるよう、あらかじめ設定されています。単一の Astro アプリ用に、Node.js、プロジェクトの依存関係、エンドツーエンドテスト用のブラウザーバイナリ、マイグレーションと初期データ投入が済んだ SQLite データベースを利用できます。

```yaml
name: "Copilot Setup Steps"

# リポジトリの "Actions" タブからセットアップ手順をテストできます
on: workflow_dispatch

env:
  ASTRO_TELEMETRY_DISABLED: "1"

jobs:
  copilot-setup-steps:
    runs-on: ubuntu-latest
    # *各手順*に必要な最小限のアクセス許可を設定します。Copilot の操作には専用のトークンが使われます。
    permissions:
      # 依存関係のインストールなど、セットアップ手順の一部としてリポジトリをクローンする場合は、`contents: read` アクセス許可が必要です。セットアップ手順でクローンしない場合は、手順の完了後に Copilot が自動的にクローンします。
      contents: read
    steps:
      - name: Checkout code
        uses: actions/checkout@v5

      # フロントエンド / アプリのセットアップ - Node.js (アプリ全体が Astro + Drizzle/libSQL になりました)
      - name: Set up Node.js
        uses: actions/setup-node@v6
        with:
          node-version: "lts/*"
          cache: "npm"
          cache-dependency-path: "./package-lock.json"

      - name: Install JavaScript dependencies
        run: npm ci

      - name: Install Playwright browsers
        run: npx playwright install --with-deps chromium

      # ビルドとテストでデータを利用できるように、ローカルの SQLite データベースをマイグレーションして初期データを投入します。
      - name: Set up the database
        run: npm run db:setup
```

一般的な GitHub ワークフローファイルと同じように見えますが、いくつか重要な点があります。

- `copilot-setup-steps` という単一のジョブが含まれています。Copilot がプルリクエストの作業を始める前に、このジョブが GitHub Actions で実行されます。
- `workflow_dispatch` トリガーが設定されています。これを使うと、リポジトリの **Actions** タブからワークフローを手動実行できます。Copilot が実行するのを待たずに、正常に動くかテストできます。

## ドキュメントを追加する

ドキュメントの重要性は広く理解されていますが、古い情報しかないプロジェクトや、ドキュメントがまったくないプロジェクトも多くあります。このような技術的負債が放置されると、生産性が低下し、コードベースの保守や新しい開発者の受け入れが難しくなります。Copilot はドキュメントの作成に役立つため、この issue は Copilot クラウドエージェントに割り当てるのに適しています。バックグラウンドで必要なドキュメントを生成します。後の演習で結果を確認します。

1. 新しいブラウザータブで github.com のリポジトリに移動します。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して issue を作成します。
5. **Title** に `Code lacks documentation` と入力します。
6. **Description** に次の内容を入力します。

   ```plaintext
   Our organization has a requirement that functions and methods include TSDoc doc comments where helpful. Unfortunately, recent updates haven't followed this standard. We need to update the existing code to ensure doc comments are included where they clarify behavior.
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。
10. **Pull Requests** タブを選択します。
11. 新しく生成されたプルリクエスト (PR) を開きます。タイトルは `[WIP]: Code lacks documentation` のような内容です。一覧に PR が表示されない場合は、少し待ってからブラウザーを更新します。
12. 数分後、Copilot が作成した To Do リストが表示されます。

> [!NOTE]
> Copilot の To Do リストが PR に表示されるまで、数分かかることがあります。Copilot は先ほどのワークフローを実行して環境を作成し、プロジェクトを分析して、問題に取り組む方法を判断しています。

13. リストを確認し、Copilot が実行するタスクを確認します。
14. プルリクエストのタイムラインを下にスクロールし、Copilot が issue の作業を開始したことを示す更新を確認します。
15. **View session** ボタンを選択します。

   ![Copilot のセッションビュー](../../../_images/ex4-view-session.png)

> [!CAUTION]
> 表示が更新されない場合は、ブラウザーを更新してください。

16. ライブセッションをスクロールし、Copilot がどのように問題を解決しているか確認します。コードを調べて状態を理解し、方針を考えるために一時停止し、コードを作成する様子などを確認できます。

完了まで数分かかることがあります。Copilot クラウドエージェントの主な目的の1つは、タスクを非同期で実行し、開発者が他の作業に集中できるようにすることです。この利点を活かすため、別のタスクも Copilot クラウドエージェントに割り当ててから、アプリケーションの機能追加に取り組みます。

## ゲーム詳細ページに関連ゲームのセクションを追加する

GitHub Copilot クラウドエージェントの利点の1つは、作業を分けられることです。開発者があるタスクに取り組んでいる間に、エージェントは別のタスクを進められます。デザインチーム向けの関連ゲームの追加に時間がかからないとしても、その時間をほかの作業に使えます。Copilot クラウドエージェントに依頼しましょう。

1. github.com のリポジトリに戻ります。
2. **Issues** タブを選択します。
3. **New issue** を選択して、新しい issue のダイアログを開きます。
4. **Blank issue** を選択して、空のテンプレートを使います。
5. **Title** に `Show related games on the game details page` と入力します。
6. **Description** に次の内容を入力します。

   ```markdown
   We want to help people discover more games by showing related games on each game's details page. The design team wants to explore the UX and do some acceptance testing. Our requirements are:

   - Add a data-access helper in `src/lib/` that returns other games in the same category as a given game, excluding that game
   - Show a "Related games" section on the game details page that uses the helper
   - Handle the case where a game has no related games
   - There should be unit tests created for the new helper
   - Before creating the PR, ensure all tests pass
   ```

7. **Create** を選択して issue を作成します。
8. 右側の **Assign to Copilot** を選択して、割り当てダイアログを開きます。

   ![issue に Copilot を割り当てる](../../../_images/shared-assign-copilot.png)

9. **Assign** を選択します。

まもなく、Copilot が作業を始めたことを示す 👀 が issue の最初のコメントに表示されます。

![issue で Copilot が作業中であることを目の絵文字で示している](../../../_images/ex4-issue-eyes-emoji.png)

Copilot は2つ目の依頼にも取り組んでいます。Copilot クラウドエージェントはソフトウェアエンジニアのように作業するため、常に監視する必要はありません。完了後に結果をレビューします。次は、カスタムエージェントの作成と使用を確認します。

## まとめと次のステップ

このレッスンでは [GitHub Copilot クラウドエージェント][copilot-agents]について学びました。Copilot に issue を割り当て、非同期で作業させることができます。技術的負債への対処、新機能の作成、別のフレームワークへのコード移行などに利用できます。

次の内容を確認しました。

- コード生成環境のカスタマイズ。
- 操作を安全に実行する方法。
- 範囲を明確に定めた issue の重要性。
- issue を Copilot に割り当てる方法。

クラウドエージェントがバックグラウンドで作業している間に、カスタムエージェントの作成と使用に取り組みます。[Copilot クラウドエージェントは MCP サーバーも利用でき][cloud-agent-mcp]、前のモジュールで確認したカスタム指示も利用できます。

## 参考資料

- [Copilot クラウドエージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]

| [次のレッスン: カスタムエージェント →][next-lesson] |
|--:|

[previous-lesson]: ../1-custom-instructions/
[next-lesson]: ../3-custom-agents/
[cloud-agent-mcp]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/extend-cloud-agent-with-mcp
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[github-actions]: https://docs.github.com/actions
[start-copilot-sessions]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
