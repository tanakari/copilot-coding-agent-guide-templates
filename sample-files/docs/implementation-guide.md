# DDD実装ガイドライン

[プロジェクト名]プロジェクトのDDD（ドメイン駆動設計）実装における技術的ガイドラインです。

## ドメイン層 (Domain Layer)

### 基本方針
- **Pure Java**: インフラストラクチャ技術（JPA、Spring等）に依存しない
- **不変性**: バリューオブジェクトは不変オブジェクトとして実装
- **ビジネスルール**: エンティティ内にビジネスロジックを配置
- **依存関係**: 他の層への依存を持たない

### 実装対象
- エンティティ（Entity）
- バリューオブジェクト（Value Object）
- ドメインサービス
- リポジトリインターフェース（実装は別層）
- ドメインイベント（必要に応じて）

### 実装例
```java
// エンティティの例
public class [Entity] {
    private [EntityId] id;
    private [ValueObject] valueObject;
    
    // ビジネスルールをエンティティ内に実装
    public void businessLogic() {
        // ドメインルール
    }
}
```

## インフラストラクチャ層 (Infrastructure Layer)

### 基本方針
- **変換責務**: 永続化形式 ↔ ドメインオブジェクトの変換
- **依存方向**: インフラ層 → ドメイン層（逆方向禁止）
- **技術分離**: データベース技術をドメイン層から隠蔽

### 実装対象
- JPA Entity（永続化用）
- Repository実装クラス
- データベーステーブル定義（DDL）
- エンティティ変換ロジック

### 実装例
```java
// JPA Entity（ドメインエンティティとは別）
@Entity
@Table(name = "[table_name]")
public class [Entity]JpaEntity {
    // JPA専用の実装
}

// Repository実装
@Repository
public class [Entity]RepositoryImpl implements [Entity]Repository {
    // Spring Data JPAを使用した実装
}
```

## アプリケーション層 (Application Layer)

### 基本方針
- **薄い層**: ビジネスロジックはドメイン層に委譲
- **ユースケース**: 1つのサービスクラス = 1つのユースケース
- **トランザクション**: この層でトランザクション境界を管理

### 実装対象
- アプリケーションサービス
- DTO（Data Transfer Object）
- ユースケース実装

### 実装例
```java
@Service
@Transactional
public class [UseCase]Service {
    private final [Entity]Repository repository;
    
    public [Result]Dto execute([Input]Dto input) {
        // ドメイン層に処理を委譲
        // DTOとドメインオブジェクトの変換
    }
}
```

## プレゼンテーション層 (Presentation Layer)

### 基本方針
- **薄い層**: ビジネスロジックはアプリケーション層に委譲
- **HTTP境界**: HTTPリクエスト/レスポンスの処理のみ
- **バリデーション**: 入力値の形式チェック

### 実装対象
- RESTコントローラー
- リクエスト/レスポンスDTO
- 例外ハンドラー

### 実装例
```java
@RestController
@RequestMapping("/api/[resources]")
public class [Resource]Controller {
    private final [UseCase]Service service;
    
    @PostMapping
    public ResponseEntity<[Response]Dto> create(@Valid @RequestBody [Request]Dto request) {
        // アプリケーション層に処理を委譲
    }
}
```

## テスト戦略

### 単体テスト
- **ドメイン層**: ビジネスロジックの詳細テスト
- **アプリケーション層**: ユースケースのテスト
- **プレゼンテーション層**: HTTP APIのテスト

### 統合テスト
- **データベース**: TestContainers使用
- **API**: MockMvcによるE2Eテスト
- **全体フロー**: 実際のユーザーシナリオテスト

## 依存関係ルール

```
Presentation → Application → Domain ← Infrastructure
```

- ドメイン層は他の層に依存しない
- インフラ層はドメイン層のインターフェースを実装
- 上位層は下位層に依存可能（逆は禁止）

## 命名規則

### パッケージ構造
```
com.example.[project]/
├── domain/           # ドメイン層
│   ├── model/       # エンティティ・バリューオブジェクト
│   └── repository/  # リポジトリインターフェース
├── application/      # アプリケーション層
│   └── service/     # アプリケーションサービス
├── infrastructure/   # インフラストラクチャ層
│   ├── persistence/ # 永続化
│   └── config/      # 設定
└── presentation/     # プレゼンテーション層
    └── rest/        # RESTコントローラー
```

### クラス命名
- エンティティ: `[Name]`
- バリューオブジェクト: `[Name]`
- リポジトリインターフェース: `[Name]Repository`
- リポジトリ実装: `[Name]RepositoryImpl`
- アプリケーションサービス: `[UseCase]Service`
- コントローラー: `[Resource]Controller`