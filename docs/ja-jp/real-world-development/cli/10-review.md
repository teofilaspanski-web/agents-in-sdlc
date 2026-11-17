---
title: "レッスン 10 - まとめと次のステップ"
description: "Copilot CLI のワークフロー、2つのプルリクエスト、再利用可能なカスタマイズ、参考資料を振り返ります。"
authors:
  - geektrainer
lastUpdated: 2026-09-29
---

コマンドラインインターフェイス (CLI) 版の GitHub Copilot である GitHub Copilot CLI を使い、Tailspin Toys で一連の開発作業を行いました。具体的には、次のとおりです。

- Codespace を準備し、Copilot CLI をインストールして、プロジェクトを確認し、用意されている絞り込みの issue を見つけました。
- 星評価を追加し、転送されたブラウザーで結果を確認し、最初のプルリクエスト (PR) を手動でマージしました。
- 絞り込みの issue から始め、Plan モードで実装方法を決め、Autopilot モードで機能を作成し、Interactive モードで確認しました。
- カスタム指示でエージェントを導き、既存の `quality-checks` スキルをカスタマイズして、単体テスト、lint、型チェックを実行しました。
- Playwright Model Context Protocol (MCP) サーバーを追加し、実際のブラウザーで絞り込み機能を確認しました。
- 要件、テストカバレッジ、スキルの結果、ブラウザーでの検証結果を評価する品質保証 (QA) カスタムエージェントを作成して選択しました。
- 絞り込み機能全体を確認し、絞り込みの PR に Agent Merge を許可しました。
- コンテキスト、モデル、共有、任意のクラウドへの委任に使うスラッシュコマンドを確認しました。

## リリースした内容

このワークショップでは、更新済みの `main` からそれぞれブランチを作り、2つの PR を作成しました。

1. **星評価**: 既存の `starRating` と、評価がない場合の状態をゲームカードに表示します。
2. **絞り込みと品質ワークフロー**: 絞り込みを実装し、指示を更新して機能に適用し、`quality-checks` レポートをカスタマイズし、QA プロファイルを作成して、関連するテストを追加します。

絞り込み機能の計画から PR の作成まで、同じ会話とブランチを使いました。ワークショップを効率よく進めるため、これらの作業を1つの PR にまとめました。

## さまざまな検証方法

自動テスト、自分で行うブラウザー確認、MCP 経由で Copilot が行うブラウザー確認など、複数の方法でコードを確認しました。`quality-checks` スキルは単体テスト、lint、型チェックを実行し、カスタマイズした形式で結果を報告しました。QA は PR の前に、要件とテストカバレッジの確認にこれらの結果をまとめました。

追加するテストは、実際の不足を補うものである必要があります。QA の実行で新しいテストが不要と判断されることもあります。マージを許可する前に、コードと検証の根拠を確認してください。変更を加えた場合は、影響を受ける検証結果を更新します。

## ベストプラクティス

Copilot に与えるコンテキストとツールが、作業の結果を左右します。このワークショップでは、指示を更新し、スキルをカスタマイズし、QA プロファイルを作成し、MCP サーバーを構成しました。これらのカスタマイズは会話をまたいで再利用し、チームのニーズに合わせて調整できます。指示は標準を定め、スキルは繰り返し実行するタスクを記述し、カスタムエージェントは専門的な役割を定義し、MCP サーバーは外部ツールに接続します。エージェントの要約だけでなく、実際の変更とツールの結果を確認してください。

タスクに合わせて**モードとモデル**を選びます。構築前に方針を考えるには **Plan**、範囲を絞った変更に参加しながら取り組むには **Interactive**、範囲が明確なタスクには **Autopilot** を使います。定型的な編集には高速なモデルを、複雑な作業にはより高性能なモデルを選びます。

基盤と同じくらい、コンテキストも重要です。何を、なぜ、どのように作るのかを明確に説明すると、出力が大きく変わります。

## さらに確認する

主要なワークフローを学びました。Copilot CLI には、次のような機能もあります。

- `/review` でコードレビューエージェントに変更を分析させます。
- `/rubber-duck` で問題を話し合い、別の視点から意見を得ます。
- `/fleet` で独立したサブタスクを並行して調整します。
- `/worktree` で別のタスクを分離します。
- `/delegate` で Copilot クラウドエージェントにタスクを送信します。

## 次のステップ

ツールの活用を上達させるには、使い続けるのが一番です。本番コード、趣味のコード、長年作りたいと思っていた小さなアプリなどで活用してください。チームと学んだことを共有し、互いの経験から学びましょう。いつものように、ドキュメントも確認してください。

ターミナルから Tailspin Toys を拡張する場合は、任意の[Foundry Backer Concierge シリーズ][foundry]に進んでください。異なる環境を比較するには、[VS Code ワークショップ][vscode]、[GitHub Copilot アプリのワークショップ][app]、または[Copilot クラウドエージェントのワークショップ][cloud]を確認してください。

## 参考資料

- [GitHub Copilot CLI について][about-cli]
- [Copilot CLI コマンドリファレンス][cli-reference]
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
