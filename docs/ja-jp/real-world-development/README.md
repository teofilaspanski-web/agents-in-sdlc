---
title: "実践的な開発"
slug: ja-jp/real-world-development
authors:
  - geektrainer
lastUpdated: 2026-09-16
---

現実的なシナリオ、完成したアプリケーション、共有バックログを使って、ソフトウェア開発ライフサイクル全体で GitHub Copilot を活用します。変更を計画し、コードを作成し、動作をテストし、プルリクエストをレビューし、エージェントを使って作業を管理する方法を、反復的に学びます。

## シナリオ

架空の会社 Tailspin Toys に新しく加わった開発者として作業します。同社は開発者をテーマにしたボードゲームのクラウドファンディングを提供しています。チームのバックログは GitHub issue として登録済みで、すぐに取り組めます。アクセシビリティやコーディング標準などの品質改善に加えて、絞り込みやページネーションなどの機能も含まれています。

## 作業環境を選ぶ

GitHub Copilot は、開発者が作業するさまざまな環境で利用できます。希望する開発方法に合った環境を選んでください。各ワークショップは、それぞれ独自のセットアップから始まり、共通の Tailspin Toys シナリオを使います。

### [GitHub Copilot CLI][cli]

ターミナルでエージェント型アシスタントを使います。Plan モードと Autopilot モード、指示、スキル、カスタムエージェント、Playwright Model Context Protocol (MCP)、Agent Merge、実用的なスラッシュコマンドを活用します。

### [GitHub Copilot アプリ][app]

デスクトップアプリでエージェントセッションを並行して実行し、セッションモードを切り替え、キャンバスで共同作業し、GitHub issue とプルリクエストを管理します。

### [GitHub Copilot クラウドエージェント][cloud]

GitHub issue を非同期エージェントに割り当て、作業を導き、進捗を監視して、作成されたプルリクエストをレビューします。

### [Visual Studio Code][vscode]

Visual Studio Code と GitHub Codespaces で GitHub Copilot を使います。エディターを離れずに、Copilot Chat のエージェントモード、MCP サーバー、カスタムエージェントを活用します。

[cli]: cli/
[app]: app/
[cloud]: cloud/
[vscode]: vscode/
