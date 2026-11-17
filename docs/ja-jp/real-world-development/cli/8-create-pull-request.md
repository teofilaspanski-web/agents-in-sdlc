---
title: "レッスン 8 - 機能の PR を作成してマージする"
description: "絞り込み機能、指示、スキルの更新、QA プロファイル、テストをまとめて確認し、PR を作成して Agent Merge を使います。"
authors:
  - geektrainer
lastUpdated: 2026-09-18
---

絞り込み機能の実装、指示の更新、スキルの更新、品質保証 (QA) プロファイル、テストは、すべて1つのブランチに保存されています。これらをまとめて確認し、プルリクエストを開きましょう。星評価のプルリクエスト (PR) は手動でマージしましたが、今回は **Agent Merge** にプロセスを管理させます。

> [!NOTE]
> 通常は、機能、指示の更新、スキルの更新、QA エージェントを複数の PR に分けます。このワークショップでは、手順を簡潔にするため、絞り込み機能と品質ワークフローを1つの会話とブランチにまとめ、1つの PR に含めています。

このレッスンでは、次のことを行います。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びます。
- 機能全体の変更と検証の根拠を確認します。
- 絞り込み機能の PR を作成します。
- 確認後にのみ Agent Merge を有効にし、PR がマージされたことを確認します。

## シナリオ

絞り込み機能の開発では、Copilot を使って計画、実装、検証を行いました。Tailspin Toys では、マージの許可は開発者が管理しながら、残りの PR 作業を自動化したいと考えています。

## Agent Merge について

**Agent Merge** は、プルリクエストをマージ可能な状態にするための残りの作業を自動化します。有効にすると、Copilot は PR を妨げている要因に対処します。継続的インテグレーション (CI) チェックの失敗を修正し、レビューコメントに対応し、必要に応じてリベースを行った後、リポジトリが許可していれば GitHub の自動マージを有効にします。

これまでは、自分で **Merge pull request** を選択していました。Agent Merge にその作業を任せることもできます。Agent Merge を有効にする前に作業を確認し、準備ができていることを判断してください。

## Agent Merge で PR を管理する

コードの作成が完了したので、まとめて確認し、PR を作成して、残りのプロセスを Agent Merge に任せます。

1. Codespace に戻ります。
2. `/agent` を入力してエージェントダイアログを開きます。
3. 選択肢から **Default** を選び、<kbd>Enter</kbd> を押します。
4. `/pr create` コマンドを使って新しい PR を作成します。
5. `/pr agentmerge` を使って Agent Merge を有効にします。
6. Copilot が PR の継続的インテグレーションを確認します。すべて成功すると、マージが実行されます。
7. Copilot から「PR #14 was squash-merged successfully.」のようなメッセージが表示されたことを確認します。

> [!IMPORTANT]
> Agent Merge は、必須の承認、ブランチ保護、マージキュー、リポジトリ設定、権限の不足を回避しません。処理がブロックされた場合は、報告された理由を確認し、リポジトリで許可されている場合は、レビュー済みの変更を手動でマージしてください。

## まとめと次のステップ

コードの生成、テストと検証、プルリクエストのプロセスなど、開発プロセスの複数の部分を自動化しました。具体的には、次のことを行いました。

- Agent Merge とは何か、マージのライフサイクルをどのように自動化するかを学びました。
- 機能全体の変更と検証の根拠を確認しました。
- 絞り込み機能の PR を作成しました。
- 確認後に Agent Merge を有効にし、PR がマージされたことを確認しました。

次は、コンテキスト、モデル、共有、オプションのクラウドへの委任に役立つ[その他の Copilot CLI スラッシュコマンドを確認します][next-lesson]。

## 参考資料

- [Copilot CLI でプルリクエストを管理する][manage-prs]
- [Copilot CLI コマンドリファレンス][cli-reference]

[previous-lesson]: ../7-qa-agent/
[next-lesson]: ../9-cli-power-tools/
[manage-prs]: https://docs.github.com/copilot/how-tos/copilot-cli/use-copilot-cli/manage-pull-requests
[cli-reference]: https://docs.github.com/copilot/reference/copilot-cli-reference/cli-command-reference
