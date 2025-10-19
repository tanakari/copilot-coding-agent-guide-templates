# セットアップガイド

このドキュメントでは、メンバー一覧アプリケーションプロジェクトの開発環境をセットアップする手順を説明します。

## 前提条件

開発環境をセットアップする前に、以下の環境が必要です：

- Java 25（Temurin推奨）
- Maven 3.9.x
- PostgreSQL 15+
- Git
- IDE（IntelliJ IDEA、VS Code、Eclipse など）

## 1. Java 25のインストール

### Windows

1. [Eclipse Temurin](https://adoptium.net/) から Java 25 をダウンロード
2. インストーラーを実行してインストール
3. 環境変数 `JAVA_HOME` を設定
4. `PATH` に `%JAVA_HOME%\bin` を追加

### macOS

（テンプレートのため、割愛）

### 確認

```bash
java --version
javac --version
```

## 2. Mavenのインストール

### Windows

1. [Apache Maven](https://maven.apache.org/download.cgi) から最新版をダウンロード
2. 任意のディレクトリに展開
3. 環境変数 `MAVEN_HOME` を設定
4. `PATH` に `%MAVEN_HOME%\bin` を追加

### macOS

（テンプレートのため、割愛）

### 確認
```bash
mvn --version
```

## 3. PostgreSQLのインストール

### Windows
1. [PostgreSQL公式サイト](https://www.postgresql.org/download/windows/) からインストーラーをダウンロード
2. インストーラーを実行（デフォルト設定で問題ありません）
3. パスワードを設定（忘れないようにメモ）

### macOS

（テンプレートのため、割愛）

### データベースの作成

```bash
# PostgreSQLにログイン
sudo -u postgres psql

# データベースとユーザーを作成
CREATE DATABASE member_list_app;
CREATE USER member_list_user WITH PASSWORD 'your_password';
GRANT ALL PRIVILEGES ON DATABASE member_list_app TO member_list_user;
\q
```

## 4. プロジェクトのクローンと設定

```bash
# リポジトリをクローン
git clone https://github.com/tanakari/member-list-app.git
cd member-list-app

# 環境設定ファイルをコピー
cp .env.example .env
```

## 5. 環境設定ファイルの編集

`.env` ファイルを編集して、データベース接続情報を設定します：

```properties
# データベース設定
DB_HOST=localhost
DB_PORT=5432
DB_NAME=member_list_app
DB_USERNAME=member_list_user
DB_PASSWORD=your_password

# アプリケーション設定
SERVER_PORT=8080
SPRING_PROFILES_ACTIVE=development

# ログレベル
LOGGING_LEVEL_ROOT=INFO
LOGGING_LEVEL_COM_EXAMPLE=DEBUG
```

## 6. 依存関係のインストールとアプリケーションの起動

```bash
# 依存関係をインストール
mvn clean install

# アプリケーションを起動
mvn spring-boot:run
```

## 7. 動作確認

ブラウザで以下のURLにアクセスして、アプリケーションが正常に起動していることを確認します：

- メインページ: http://localhost:8080
- ヘルスチェック: http://localhost:8080/actuator/health

## IDE設定

VS Code を前提とします。

1. プロジェクトフォルダを開く
2. 必要な拡張機能をインストール：
   - Extension Pack for Java
   - Spring Boot Extension Pack
   - Lombok Annotations Support for VS Code
3. `.vscode/settings.json` を設定

## トラブルシューティング

### ポート競合エラー

```bash
# ポート使用状況を確認
netstat -ano | findstr :8080  # Windows
lsof -i :8080                 # macOS/Linux

# 別のポートを使用する場合は.envファイルでSERVER_PORTを変更
```

### データベース接続エラー

- PostgreSQLサービスが起動しているか確認
- データベース名、ユーザー名、パスワードが正しいか確認
- ファイアウォール設定を確認

### Java バージョンエラー

```bash
# 現在のJavaバージョンを確認
java --version

# JAVA_HOME環境変数を確認
echo $JAVA_HOME  # macOS/Linux
echo %JAVA_HOME% # Windows
```

## 開発時の便利なコマンド

```bash
# アプリケーションを開発モードで起動（ホットリロード有効）
mvn spring-boot:run -Dspring-boot.run.profiles=development

# テストの実行
mvn test

# コードフォーマット
mvn com.coveo:fmt-maven-plugin:format

# 静的解析
mvn checkstyle:check

# パッケージの作成
mvn clean package
```

## 次のステップ

セットアップ完了後は、以下のドキュメントを参照してください：

- [開発フロー](./docs/development-flow.md) - 開発の進め方
- [技術スタック](./docs/stack.md) - 使用技術の詳細
- [アーキテクチャ](./docs/architecture.md) - システム設計
- [仕様書](./SPEC.md) - 機能仕様

## サポート

セットアップで問題が発生した場合は、GitHubのIssueで報告してください。
