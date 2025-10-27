# 指示書

本プロジェクトは **AI駆動開発** を想定しています。  
Copilot や AI エージェント／開発者が迷わず作業できるよう、**入口（索引）**として本ファイルを配置します。

## 🚀 初めての方へ（クイックスタート）

**AIエージェントにすぐ依頼したい場合**は、以下の最も簡単なテンプレートを使ってください：

```
[機能名]の[レイヤー]を実装するIssueを作成してください。

仕様: specs/[ファイルパス]
```

👉 **詳しい依頼方法・例文**: [`docs/prompts-examples.md`](./docs/prompts-examples.md)  
👉 **AIとの作業ルール**: [`docs/ai/agent.md`](./docs/ai/agent.md)

---

## 開発ルール・手順

- AIエージェント作業契約: [`docs/ai/agent.md`](./docs/ai/agent.md)
- 開発フロー: [`docs/development-flow.md`](./docs/development-flow.md)
- **Pull Request作成**: 必ず[PRテンプレート](`.github/pull_request_template.md`)を完全に記載してください
- 命名・コーディング規約: [`docs/conventions.md`](./docs/conventions.md)
- アーキテクチャ指針: [`docs/architecture.md`](./docs/architecture.md)
- 注意事項/ポリシー: [`docs/policies.md`](./docs/policies.md)
- AIエージェントへの依頼例: [`docs/prompts-examples.md`](./docs/prompts-examples.md)

### 🔥 重要: Pull Request作成時の必須ルール

**AIエージェント向け技術指示:**
- `.github/pull_request_template.md` の**全項目**を必ず記載してください
- **詳細ルール**: [`docs/ai/agent.md`](./docs/ai/agent.md) の「PR作成強制ルール」を厳守

**技術的実装手順:**
1. コード実装完了後、テストを実行
2. 【重要】agent.mdのPR作成ルールに従って実行
3. PRテンプレートが自動生成されることを確認
4. **すべてのセクション**が適切に埋められていることを検証
5. 最終確認後にPR作成
3. `<!-- ✅ 必須: ... -->` の項目をすべて記載
4. **完了チェックリスト**の全項目をチェック
5. **テスト結果詳細**も必須で記載
6. 最終確認後にPR作成

## 仕様書

- 仕様概要・一覧: [`docs/SPEC.md`](./docs/SPEC.md)
- 仕様書ディレクトリ: [`docs/specs/`](./docs/specs/)