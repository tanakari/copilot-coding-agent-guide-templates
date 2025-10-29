# DDD実装ガイドライン

[プロジェクト名]プロジェクトのDDD（ドメイン駆動設計）実装における技術的ガイドラインです。

> このファイルはプロジェクト開始時にあなたのドメインに合わせてカスタマイズしてください。

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

// バリューオブジェクトの例
public final class [ValueObject] {
    private final String value;
    
    public [ValueObject](String value) {
        // バリデーション
        this.value = value;
    }
    
    // 不変オブジェクト
    public String getValue() { return value; }
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
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(name = "[column_name]")
    private String [property];
    
    // JPA専用の実装（ドメインロジックなし）
}

// Repository実装
@Repository
public class [Entity]RepositoryImpl implements [Entity]Repository {
    private final [Entity]JpaRepository jpaRepository;
    
    @Override
    public Optional<[Entity]> findById([EntityId] id) {
        return jpaRepository.findById(id.getValue())
            .map(this::convertToDomain);
    }
    
    private [Entity] convertToDomain([Entity]JpaEntity jpaEntity) {
        // JPA Entity → ドメインエンティティ変換
    }
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
        // 1. DTOからドメインオブジェクトに変換
        var domainObject = convertToDomain(input);
        
        // 2. ドメイン層に処理を委譲
        var result = domainObject.businessLogic();
        
        // 3. ドメインオブジェクトからDTOに変換
        return convertToDto(result);
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
        var result = service.execute(request);
        return ResponseEntity.ok(result);
    }
    
    @GetMapping("/{id}")
    public ResponseEntity<[Response]Dto> findById(@PathVariable Long id) {
        // アプリケーション層に処理を委譲
    }
}

// Global Exception Handler
@RestControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler([Domain]Exception.class)
    public ResponseEntity<ErrorResponse> handleDomainException([Domain]Exception e) {
        // ドメイン例外をHTTPレスポンスに変換
    }
}
```

## テスト戦略

### 単体テスト
- **ドメイン層**: ビジネスロジックの詳細テスト（Pure Java）
- **アプリケーション層**: ユースケースのテスト（モック使用）
- **プレゼンテーション層**: HTTP APIのテスト（@WebMvcTest）

### 統合テスト
- **データベース**: TestContainers使用
- **API**: MockMvcによるE2Eテスト
- **全体フロー**: 実際のユーザーシナリオテスト

### テスト例
```java
// ドメイン層テスト
class [Entity]Test {
    @Test
    void shouldExecuteBusinessLogic() {
        // Pure Javaでのビジネスロジックテスト
    }
}

// 統合テスト
@SpringBootTest
@Testcontainers
class [Entity]IntegrationTest {
    @Container
    static PostgreSQLContainer<?> postgres = new PostgreSQLContainer<>("postgres:15");
    
    @Test
    void shouldPersistEntity() {
        // データベース統合テスト
    }
}
```

## 依存関係ルール

```
Presentation → Application → Domain ← Infrastructure
```

- **ドメイン層**: 他の層に依存しない（Pure Java）
- **インフラ層**: ドメイン層のインターフェースを実装
- **上位層**: 下位層に依存可能（逆は禁止）
- **依存性注入**: Spring DIでインフラ実装を注入

## 命名規則

### パッケージ構造
```
com.example.[project]/
├── domain/           # ドメイン層
│   ├── model/       # エンティティ・バリューオブジェクト
│   ├── service/     # ドメインサービス
│   └── repository/  # リポジトリインターフェース
├── application/      # アプリケーション層
│   ├── service/     # アプリケーションサービス
│   └── dto/         # DTO定義
├── infrastructure/   # インフラストラクチャ層
│   ├── persistence/ # 永続化
│   └── config/      # 設定
└── presentation/     # プレゼンテーション層
    ├── rest/        # RESTコントローラー
    └── dto/         # リクエスト/レスポンスDTO
```

### クラス命名
- **エンティティ**: `[Name]` (例: `User`, `Order`)
- **バリューオブジェクト**: `[Name]` (例: `Email`, `Money`)
- **リポジトリインターフェース**: `[Name]Repository`
- **リポジトリ実装**: `[Name]RepositoryImpl`
- **アプリケーションサービス**: `[UseCase]Service` (例: `CreateUserService`)
- **コントローラー**: `[Resource]Controller` (例: `UserController`)
- **DTO**: `[Purpose]Dto` (例: `CreateUserRequestDto`, `UserResponseDto`)

## GitHub Copilot活用のためのTips

### 1. 明確なコメント
```java
// [Entity]エンティティの[Business Rule]実装
public class [Entity] {
    // [Validation Rule]をチェック
    public void validate() {
        // 実装をCopilotに任せる
    }
}
```

### 2. テスト駆動開発
```java
@Test
void should[期待する動作]_when[前提条件]() {
    // Given
    // When  
    // Then - Copilotが実装をサポート
}
```

### 3. インターフェース優先設計
```java
// インターフェースを先に定義してCopilotに実装してもらう
public interface [Name]Repository {
    Optional<[Entity]> findBy[Criteria]([Type] criteria);
    void save([Entity] entity);
}
```