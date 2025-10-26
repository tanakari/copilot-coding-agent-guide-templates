# [プロジェクト名]

## 概要

[プロジェクトの説明を記載してください]

このアプリケーションはAI駆動で開発しています。

## 🤖 AIエージェントとの開発

**開発は AI駆動で行います。** 詳細な開発ルールは [`INSTRUCTIONS.md`](./INSTRUCTIONS.md) を参照してください。

### 📋 基本的な依頼方法

```
[機能名]の[レイヤー]を実装するIssueを作成してください。

仕様: specs/[ファイルパス]
```

**例:** `[機能名]の[API/UI]を実装してください。仕様: specs/[ファイルパス]`

### 🔗 AI開発関連ドキュメント
- **開発指示書**: [`INSTRUCTIONS.md`](./INSTRUCTIONS.md) - AIエージェント向け詳細ルール
- **依頼例文集**: [`docs/prompts-examples.md`](./docs/prompts-examples.md) - 具体的な依頼方法
- **AI作業契約**: [`docs/ai/agent.md`](./docs/ai/agent.md) - AIとの協働ルール

### ⚠️ 重要ルール
- **PR作成時**: `.github/pull_request_template.md` の**全項目記載必須**
- **開発前**: 必ず [`INSTRUCTIONS.md`](./INSTRUCTIONS.md) を確認

## 主な機能

- メンバー情報の登録・更新・削除
- メンバー情報の一覧表示
- メンバー情報の検索機能（名前・メール）
- ページング機能

## アーキテクチャ・技術

### アーキテクチャ

- ドメイン駆動

### 技術

- Java 25
- Spring Boot 3.5.x
- Thymeleaf
- Spring Data JPA
- Maven
- PostgreSQL

詳細な技術スタックは [docs/stack.md](./docs/stack.md) を参照してください。

## セットアップ

セットアップ手順は [SETUP.md](./SETUP.md) を参照してください。

### クイックスタート

```bash
# リポジトリクローン
git clone https://github.com/tanakari/member-management.git
cd member-management

# 環境設定
cp .env.example .env
# .envファイルを編集

# 依存関係インストール & 起動
mvn clean install
mvn spring-boot:run
```

## ディレクトリ構成

```
member-management/
├── .github/              # GitHub設定
│   ├── ISSUE_TEMPLATE/  # Issueテンプレート
│   └── pull_request_template.md  # PRテンプレート
├── docs/                 # 各種ドキュメント
├── instructions/         # 開発手順書
├── specs/                # 仕様書
│   ├── api/             # API仕様書
│   └── ui/              # 画面仕様書
├── src/                  # ソースコード
│   ├── main/
│   │   ├── java/        # Javaソースコード
│   │   └── resources/   # リソースファイル
│   └── test/            # テストコード
├── pom.xml              # Maven設定ファイル
├── INSTRUCTIONS.md       # 開発手順書の目次（AI向け指示書）
├── SPEC.md              # 仕様書の目次
├── SETUP.md             # セットアップガイド
└── README.md            # このファイル
```
