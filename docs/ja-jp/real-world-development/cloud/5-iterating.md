---
title: "演習 5: GitHub Copilot の作業を反復する"
authors:
  - geektrainer
lastUpdated: 2026-06-30
---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

## 作業をレビューする

このラボでは、ユーザーエクスペリエンスを改善し、機能を追加する複数のタスクで GitHub Copilot を使いました。コードへのドキュメント追加、デザインチームが試せる関連ゲーム機能の作成、ハイコントラストとライトモードの切り替え機能を含むアクセシビリティ機能の実装を依頼しました。次はコードの変更を確認し、必要に応じて Copilot にフィードバックを送って作業を改善します。

### シナリオ

これまでに何度も確認したとおり、生成 AI を導入しても、ソフトウェア設計と DevOps の基本は変わりません。生成されたコードをレビューし、通常の DevOps プロセスに沿って作業します。GitHub Copilot が提案したドキュメント、関連ゲーム機能、アクセシビリティ機能をレビューしてから、チームのレビューを開始します。

## セキュリティと GitHub Copilot クラウドエージェント

Copilot クラウドエージェントは非同期かつ人間の監視なしで作業するため、安全性を保つためのセキュリティ上の制約があります。

- Copilot はリポジトリへの読み取りアクセス権を持ち、書き込みアクセス権はコードの作業に使うブランチ**のみ**に限定されます。
- クラウドエージェントは GitHub Actions 内で実行され、作業用に分離された一時的な環境を作成します。
- GitHub Actions ワークフローを実行するには、人間による承認が必要です。
- Model Context Protocol (MCP) サーバーを含む[外部リソースへのアクセスは、既定で制限されています](https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall)。

## 生成されたドキュメントをレビューする

まず、GitHub Copilot クラウドエージェントが作成した最初のプルリクエスト (PR)、コードへのドキュメント追加を確認します。github.com の標準的な PR インターフェイスを使います。

> [!NOTE]
> PR を確認するときに、GitHub Copilot がファイアウォールでブロックされたという警告が表示される場合があります。Copilot は MCP サーバーなどの外部リソースへのアクセスが既定で制限されているため、これは**想定どおりの動作**です。必要に応じて、[Copilot クラウドエージェントのファイアウォールをカスタマイズまたは無効化][agent-firewall]できます。

1. github.com のリポジトリに戻ります。
2. **Pull Requests** を選択してプルリクエストの一覧を開きます。
3. **Add missing documentation** など、同様のタイトルが付いたプルリクエストを開きます。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。表示されている場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。休憩したり、これまでに学んだことを振り返ったりしてください。

4. プルリクエストの準備ができたら、**Files changed** タブを選択し、変更を確認します。

   ![Files changed タブ](../../../_images/shared-pr-files-changed.png)

5. 新しく追加された TSDoc のドキュメントコメントなど、更新されたコードを確認します。具体的な変更内容は状況によって異なります。

   変更を確認するときは、TSDoc のドキュメントコメント、TypeScript の規約、ファイルヘッダーコメントを探します。これらは、演習 2 の最初に確認したカスタム指示ファイルで指定された内容です。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されたことを確認します。正常に完了すれば、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。

## GitHub Copilot に変更を依頼する

プルリクエストで Copilot と作業するのは、一方向のやりとりだけではありません。チームメンバーと同じように、プルリクエストやコードへのインラインコメントで Copilot にメンションできます。Copilot はコメントを確認し、対応する新しいセッションを開始します。結果は確定的ではないため、必ず同じ結果になる具体的な文面は提示できません。Copilot に依頼できる変更の例を挙げます。

- 各コードファイルの先頭に、内容の簡単な説明をコメントとして追加する。
- TypeScript と Astro のファイルに TSDoc のドキュメントコメントを追加する。
- Astro アプリの構造を説明する README を作成する。

1. 上記の例などを参考に、コードベースに追加したいドキュメントの変更をコメントで依頼し、他のユーザーと同じように **@copilot** をメンションします。
2. **View Session** を選択し、Copilot の作業を確認します。Copilot が変更用の新しいセッションを開始します。
3. **Back to pull request** を選択すると、プルリクエストに戻れます。

   ![プルリクエストに戻る](../../../_images/ex7-back-to-pr.png)

4. Copilot が変更を完了すると、プルリクエストに新しいコミットが表示されます。
5. **Files changed** タブを選択して変更を確認します。

満足するまで、繰り返し Copilot に変更を依頼できます。問題がなければ、ドラフト PR をレビュー可能な状態に変更して `main` ブランチにマージします。

![PR をレビュー可能な状態に変更する](../../../_images/ex7-ready-for-review.png)

## 関連ゲーム機能をレビューする

ゲーム詳細ページに関連ゲームを表示する issue を解決するため、Copilot が作成した PR に戻ります。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Show related games on the game details page** など、同様のタイトルが付いた PR を選択します。
4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
6. ワークフローの承認待ちを示す表示を確認します。
7. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

8. プルリクエストのチェックセクションにワークフローが登録されたことを確認します。正常に完了すれば、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
9. **任意:** codespace でこのブランチに切り替えて、関連ゲーム機能を手動テストできます。codespace に移動してターミナルを開き、Copilot が作成したブランチ名 (例: **copilot/fix-8**) を `<branch-name>` に指定して、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

Copilot が関連ゲーム機能を作成しました。これまでと同様に、Copilot クラウドエージェントに変更を依頼して反復できます。たとえば、表示する関連ゲームの数を調整したり、コメントヘッダーと TSDoc のドキュメントコメントを追加したりできます (この issue はカスタム指示の更新**前**に割り当てたことを思い出してください)。**Conversation** タブに新しいコメントを追加すると、Copilot が確認して新しいセッションを開始します。

## アクセシビリティ機能をレビューする

最後に、アクセシビリティ用カスタムエージェントが実装した機能を確認します。この PR には、演習 3 で割り当てたハイコントラストモードと、演習 4 でミッションコントロールから依頼したライトモードの両方が含まれているはずです。

1. GitHub.com のリポジトリに戻ります。
2. **Pull Requests** タブを選択します。
3. **Add high contrast mode to website** など、同様のタイトルが付いた PR を選択します。

> [!NOTE]
> Copilot がまだ作業中の場合、プルリクエストのタイトルに **[WIP]** が付いています。その場合は、Copilot が作業を完了するまで待ちます。数分かかることがあります。

4. **Files changed** タブを選択し、生成されたコードを確認します。
5. 実装をレビューし、特に次の点を確認します。
   - モード切り替え用の UI コンポーネント。
   - ユーザー設定を保存するローカルストレージの使用方法。
   - ハイコントラストモードとライトモードの CSS またはスタイルの変更。
   - アクセシビリティ属性 (Accessible Rich Internet Applications (ARIA) ラベル、キーボード操作など)。
   - モード切り替えを管理する JavaScript/TypeScript コード。

6. 更新内容を確認して問題がなければ、**Conversation** タブに戻り、下にスクロールします。
7. ワークフローの承認待ちを示す表示を確認します。
8. ワークフローが承認待ちの場合は、**Approve and run workflows** を選択します。

   ![ワークフローを承認して実行する](../../../_images/shared-approve-workflows.png)

9. プルリクエストのチェックセクションにワークフローが登録されたことを確認します。正常に完了すれば、単一の Astro アプリのプロジェクトチェックが成功します。完了まで数分かかることがあります。
10. **任意:** codespace でこのブランチに切り替えて、アクセシビリティ機能を手動テストできます。codespace に移動してターミナルを開き、Copilot が作成したブランチ名を `<branch-name>` に指定して、次のコマンドを実行します。

   ```bash
   git fetch origin
   git checkout <branch-name>
   ```

   その後、アプリケーションを起動し、ブラウザーでハイコントラストとライトモードの切り替えをテストします。ページを再読み込みしても設定が保持されることを確認します。

アクセシビリティ用カスタムエージェントが、アクセシビリティのベストプラクティスに沿って Copilot の作業を導いたことを確認してください。アクセシビリティに関する懸念や改善点があれば、前の PR と同様に、コメントで **@copilot** に更新を依頼できます。

## 任意の演習 - さらにタスクを委任する

クラウドエージェントは、実際のバックログ項目を渡して別の作業に取り組めるときに役立ちます。習慣を身につけるために、リポジトリに issue を追加して Copilot に割り当ててみてください。例を挙げます。

- ゲーム詳細ページに支援者向け問い合わせフォームを作成します。
- ゲーム一覧ページにページネーションを実装します。
- データアクセスヘルパーに入力検証とエラー処理を追加します。

## まとめ

クラウドエージェントハーネスを完了しました。これらのレッスンでは、次のことを行いました。

- **このリポジトリに含まれるカスタム指示ファイルを確認しました。** クラウドエージェントが生成する結果への影響を確認できるようにしました。
- **issue を Copilot クラウドエージェントに割り当てました。** 環境のセットアップ、計画、非同期の実行を確認しました。
- **アクセシビリティ用のカスタムエージェントを作成して使いました。** ハイコントラストとライトモードの切り替え機能を追加しました。
- **Copilot エージェントページをミッションコントロールとして使いました。** アクセシビリティのセッションを監視し、途中で方向を調整しました。
- **クラウドエージェントのプルリクエストをレビューして反復しました。** `@copilot` に変更を依頼し、ワークフローを承認しました。

## 振り返りと次のステップ

クラウドエージェントハーネスを完了しました。さらに学びたい場合は、他のハーネスで今回の内容を補完できます。

- 🖥️ **[VS Code ハーネス](../../vscode/)** — IDE から Copilot Chat のエージェントモードと MCP の統合を確認します。
- 💻 **[CLI ハーネス](../../cli/)** — Plan と Autopilot モード、エージェントスキル、QA カスタムエージェント、Playwright MCP、Agent Merge、クラウドエージェントに戻る任意の `/delegate` を使い、ターミナルからレビュー済みの変更を2つリリースします。

自分のリポジトリでは、次のアイデアを試せます。

- リファクタリングやテストカバレッジの issue をクラウドエージェントに割り当てます。
- 別の分野のカスタムエージェントを作成します。
- 別の技術スタック向けに `copilot-setup-steps.yml` を設定します。

## 参考資料

- [GitHub Copilot][github-copilot]
- [Copilot エージェントについて][copilot-agents]
- [GitHub issue を Copilot に割り当てる][assign-issue]
- [Copilot クラウドエージェントのセットアップワークフローのベストプラクティス][cloud-agent-best-practices]
- [Copilot クラウドエージェントのファイアウォールを構成する][agent-firewall]

---

| [← 前のレッスン: エージェントの管理][previous-lesson] |
|:--|

[github-copilot]: https://github.com/features/copilot
[cloud-agent-overview]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[assign-issue]: https://docs.github.com/copilot/how-tos/use-copilot-agents/cloud-agent/start-copilot-sessions
[setup-workflow]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[copilot-agents]: https://docs.github.com/copilot/concepts/agents/cloud-agent/about-cloud-agent
[cloud-agent-best-practices]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-environment
[agent-firewall]: https://docs.github.com/copilot/how-tos/copilot-on-github/customize-copilot/customize-cloud-agent/customize-the-agent-firewall

[previous-lesson]: ../4-managing-agents/
