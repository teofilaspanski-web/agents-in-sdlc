---
title: "レッスン 10 - まとめと次のステップ"
description: "アプリのワークフロー、2つのプルリクエスト、キャンバスの演習、再利用可能な品質プラクティスを振り返り、参考資料を確認します。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

GitHub Copilot アプリを使い、Tailspin Toys で一連の開発作業を行いました。具体的には、次のことを行いました。

- リポジトリを接続し、アプリのワークスペースと用意されているバックログを確認して、クイックチャットを試しました。
- 星評価の作業に絞ったセッションを開始し、ブラウザーキャンバスで結果を確認して、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込みの issue から始め、**Plan** モードで実装方法を決め、**Autopilot** モードで機能を作成し、**Interactive** モードで確認しました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 要件、テストカバレッジ、スキルの結果、ブラウザーでの検証結果を評価する品質保証 (QA) カスタムエージェントを作成して選択しました。
- 絞り込み機能全体を確認し、2つ目の PR に **Agent Merge** を許可しました。
- 既存の Database Explorer キャンバスを使い、リポジトリのデータに基づくトリアージ用キャンバスを作成してテストしました。

## リリースした内容

このワークショップでは、更新済みの `main` からそれぞれブランチを作り、2つの PR を作成しました。

1. **星評価**: 既存の `starRating` と、評価がない場合の状態をゲームカードに表示します。
2. **絞り込みと品質ワークフロー**: 絞り込みを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

絞り込み機能の計画から PR の作成まで、同じセッション、ワークツリー、ブランチを使いました。ワークショップを効率よく進めるため、この作業を1つの PR にまとめました。その後、既存の Database Explorer を使い、PR ワークフローを繰り返すことなく、リポジトリのデータに基づくトリアージ用キャンバスを作成しました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP 経由で Copilot が行うブラウザー確認など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA は PR の前に、要件とテストカバレッジの確認にこれらの結果をまとめました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断されることもあります。利用できないツール、実行されなかったチェック、失敗は、合格ではなくブロッカーです。マージを許可する前に、コードと検証の根拠を確認してください。変更を加えた場合は、影響を受ける検証結果を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成して、キャンバスを作成しました。これらのカスタマイズはセッションをまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続し、キャンバスは共有可能な対話型の画面を提供します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前に方針を考えるには **Plan**、範囲を絞った変更に参加しながら取り組むには **Interactive**、範囲が明確で独立したタスクに限って **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高い推論能力を備えたモデルを選びます。

基盤と同じくらい、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に説明すると、出力が大きく変わります。クイックチャットは、アイデアを本格的なセッションに持ち込む前に範囲を決めるのに役立ちます。

## さらに確認する

主要なワークフローを学びました。次の機能も確認してみてください。

- [**Automations**][using-automations]: 最近の作業の要約など、定期的または必要に応じて実行するタスクに使えます。採用する前に、スケジュール、権限、範囲を確認してください。オートメーションの作成は次のステップであり、このワークショップには含まれません。
- **Rubber duck**: 作業を始める前に問題を話し合い、要点を押さえたフィードバックを得ます。
- [`/chronicle`][chronicle]: セッションで起きたことの説明を生成します。
- [Bring your own key (BYOK)][byok]: Ollama、Foundry Local、LM Studio 経由のローカルモデルなど、自分のプロバイダーのモデルを使います。
- [ディープリンク][deep-links]: アプリを開き、リポジトリ、セッション、プロンプトを直接表示します。

## 次のステップ

ツールの活用を上達させるには、使い続けるのが一番です。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。いつものように、ドキュメントも確認してください。

GitHub Copilot のエコシステムをさらに確認するには、[VS Code ハーネス][vscode-harness]、[Copilot CLI ハーネス][cli-harness]、または[クラウドエージェントハーネス][cloud-harness]を確認してください。

## 参考資料

- [GitHub Copilot アプリについて][about-copilot-app]
- [GitHub Copilot アプリを使い始める][getting-started]
- [GitHub Copilot アプリをカスタマイズする][customize]
- [オートメーションを使う][using-automations]
- [キャンバス拡張機能を使う][canvas-docs]

[vscode-harness]: ../../vscode/
[cli-harness]: ../../cli/
[cloud-harness]: ../../cloud/
[about-copilot-app]: https://docs.github.com/copilot/concepts/agents/github-copilot-app
[getting-started]: https://docs.github.com/copilot/how-tos/github-copilot-app/getting-started
[customize]: https://docs.github.com/copilot/how-tos/github-copilot-app/customize-github-copilot-app
[using-automations]: https://docs.github.com/copilot/how-tos/github-copilot-app/using-automations
[canvas-docs]: https://docs.github.com/copilot/how-tos/github-copilot-app/working-with-canvas-extensions
[chronicle]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/chronicle
[byok]: https://docs.github.com/copilot/how-tos/github-copilot-app/use-byok-models
[deep-links]: https://docs.github.com/copilot/how-tos/github-copilot-app/open-with-deep-links
