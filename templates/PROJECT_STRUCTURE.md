# プロジェクト構造

AI駆動開発プロジェクトの標準ディレクトリ構造です。

```
project-root/
├── README.md                    # プロジェクト概要
├── INSTRUCTIONS.md              # AI指示書
├── SPEC.md                      # 仕様書一覧
├── PROJECT_STRUCTURE.md         # このファイル
├── .editorconfig               # エディタ設定
├── docs/                       # ドキュメント
├── instructions/               # AI作業契約
├── specs/                      # 仕様書（要書き換え）
│   ├── api/                   # API仕様
│   ├── ui/                    # UI仕様
│   └── db/                    # DB仕様
├── .github/ISSUE_TEMPLATE/     # Issueテンプレート
└── src/                       # ソースコード（実装時作成）
```

## ファイル配置ルール

### ドキュメント
- `docs/` - 開発プロセス関連
- `instructions/` - AI作業契約
- `specs/` - 仕様書（プロジェクト固有に書き換え）

### 実装
- `src/main/java/` - Javaソースコード
- `src/test/java/` - テストコード
- `src/main/resources/` - 設定ファイル・テンプレート

## 命名規則
- ファイル名: `kebab-case.md`
- ディレクトリ名: `lowercase`
- API仕様: `create.md`, `list.md`, `update.md`, `delete.md`

## specs/ ディレクトリについて
⚠️ `specs/` 内のファイルはテンプレートです。プロジェクト固有の仕様に書き換えてください。

## 実装パッケージ構成 (Java)
```
src/main/java/com/example/project/
├── Application.java            # メインクラス
├── controller/                 # REST API
├── service/                    # ビジネスロジック
├── repository/                 # データアクセス
└── domain/                     # ドメインモデル
```