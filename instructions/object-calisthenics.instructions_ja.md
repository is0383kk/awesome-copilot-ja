---
applyTo: "**/*.{cs,ts,java}"
description: ビジネスドメインコードでクリーンで保守しやすく堅牢なコードを確実にするためのオブジェクト指向体操原則を強制する
---

# オブジェクト指向体操のルール

> ⚠️ **警告:** このファイルには 9 つの元々のオブジェクト指向体操ルールが含まれています。追加のルールは追加してはならず、これらのルールを置き換えたり削除したりしてはいけません。
> 必要に応じて例を後で追加することができます。

## 目的

このルールは、バックエンドの**主にビジネスドメインコード**においてクリーンで保守しやすく堅牢なコードを確実にするためのオブジェクト指向体操の原則を強制します。

## 適用範囲と範囲

- **主要焦点**: ビジネスドメインクラス（集約、エンティティ、値オブジェクト、ドメインサービス）
- **二次焦点**: アプリケーション層サービスとユースケースハンドラー
- **適用除外**:
  - DTO（データ転送オブジェクト）
  - API モデル/コントラクト
  - 設定クラス
  - ビジネスロジックのないシンプルなデータコンテナ
  - 柔軟性が必要なインフラストラクチャコード

## 主要原則

1. **メソッドあたり 1 レベルのインデント**:

   - メソッドがシンプルで、1 レベル以上のインデントを超えないことを確実にする。

   ```csharp
   // 悪い例 - このメソッドには複数レベルのインデントがある
   public void SendNewsletter() {
         foreach (var user in users) {
            if (user.IsActive) {
               // 何かする
               mailer.Send(user.Email);
            }
         }
   }
   // 良い例 - インデントを減らすためにメソッドを抽出
   public void SendNewsletter() {
       foreach (var user in users) {
           SendEmail(user);
       }
   }
   private void SendEmail(User user) {
       if (user.IsActive) {
           mailer.Send(user.Email);
       }
   }

   // 良い例 - メール送信前にユーザーをフィルタリング
   public void SendNewsletter() {
       var activeUsers = users.Where(user => user.IsActive);

       foreach (var user in activeUsers) {
           mailer.Send(user.Email);
       }
   }
   ```

2. **ELSE キーワードを使わない**:

   - 複雑さを減らし可読性を向上させるために`else`キーワードの使用を避ける。
   - 代わりに早期リターンを使用して条件を処理する。
   - Fail Fast の原則を使用する
   - ガード節を使用してメソッドの開始時に入力と条件を検証する。

   ```csharp
   // 悪い例 - elseを使用
   public void ProcessOrder(Order order) {
       if (order.IsValid) {
           // 注文を処理
       } else {
           // 無効な注文を処理
       }
   }
   // 良い例 - elseを避ける
   public void ProcessOrder(Order order) {
       if (!order.IsValid) return;
       // 注文を処理
   }
   ```

   Fail fast の原則のサンプル:

   ```csharp
   public void ProcessOrder(Order order) {
       if (order == null) throw new ArgumentNullException(nameof(order));
       if (!order.IsValid) throw new InvalidOperationException("Invalid order");
       // 注文を処理
   }
   ```

3. **すべてのプリミティブと文字列をラップ**:

   - コードでプリミティブ型を直接使用することを避ける。
   - 意味のあるコンテキストと動作を提供するためにクラスでラップする。

   ```csharp
   // 悪い例 - プリミティブ型を直接使用
   public class User {
       public string Name { get; set; }
       public int Age { get; set; }
   }
   // 良い例 - プリミティブをラップ
   public class User {
       private string name;
       private Age age;
       public User(string name, Age age) {
           this.name = name;
           this.age = age;
       }
   }
   public class Age {
       private int value;
       public Age(int value) {
           if (value < 0) throw new ArgumentOutOfRangeException(nameof(value), "Age cannot be negative");
           this.value = value;
       }
   }
   ```

4. **ファーストクラスコレクション**:
   - 生のデータ構造を露出するのではなく、データと動作をカプセル化するためにコレクションを使用する。
     ファーストクラスコレクション: 配列を属性として含むクラスは、他の属性を含んではならない

```csharp
   // 悪い例 - 生のコレクションを露出
   public class Group {
      public int Id { get; private set; }
      public string Name { get; private set; }
      public List<User> Users { get; private set; }

      public int GetNumberOfUsersIsActive() {
         return Users
            .Where(user => user.IsActive)
            .Count();
      }
   }

   // 良い例 - コレクションの動作をカプセル化
   public class Group {
      public int Id { get; private set; }
      public string Name { get; private set; }

      public GroupUserCollection userCollection { get; private set; } // ユーザーのリストはクラスにカプセル化される

      public int GetNumberOfUsersIsActive() {
         return userCollection
            .GetActiveUsers()
            .Count();
      }
   }
```

5. **1 行あたり 1 つのドット**:

   - 可読性と保守性を向上させるために、単一行でのメソッド呼び出しの数を制限する。

   ```csharp
   // 悪い例 - 単一行で複数のドット
   public void ProcessOrder(Order order) {
       var userEmail = order.User.GetEmail().ToUpper().Trim();
       // userEmailで何かする
   }
   // 良い例 - 1行あたり1つのドット
   public void ProcessOrder(Order order) {
       var user = order.User;
       var email = user.GetEmail();
       var userEmail = email.ToUpper().Trim();
       // userEmailで何かする
   }
   ```

6. **省略しない**:

   - クラス、メソッド、変数に意味のある名前を使用する。
   - 混乱を招く可能性のある省略を避ける。

   ```csharp
   // 悪い例 - 省略された名前
   public class U {
       public string N { get; set; }
   }
   // 良い例 - 意味のある名前
   public class User {
       public string Name { get; set; }
   }
   ```

7. **エンティティを小さく保つ（クラス、メソッド、名前空間またはパッケージ）**:

   - コードの可読性と保守性を向上させるために、クラスとメソッドのサイズを制限する。
   - 各クラスは単一の責任を持ち、可能な限り小さくする。

   制約:

   - クラスあたり最大 10 メソッド
   - クラスあたり最大 50 行
   - パッケージまたは名前空間あたり最大 10 クラス

   ```csharp
   // 悪い例 - 複数の責任を持つ大きなクラス
   public class UserManager {
       public void CreateUser(string name) { /*...*/ }
       public void DeleteUser(int id) { /*...*/ }
       public void SendEmail(string email) { /*...*/ }
   }

   // 良い例 - 単一責任の小さなクラス
   public class UserCreator {
       public void CreateUser(string name) { /*...*/ }
   }
   public class UserDeleter {
       public void DeleteUser(int id) { /*...*/ }
   }

   public class UserUpdater {
       public void UpdateUser(int id, string name) { /*...*/ }
   }
   ```

8. **2 つ以上のインスタンス変数を持つクラスは作らない**:

   - インスタンス変数の数を制限することでクラスが単一の責任を持つよう促す。
   - シンプルさを維持するためにインスタンス変数の数を 2 つに制限する。
   - ILogger やその他のロガーはインスタンス変数としてカウントしない。

   ```csharp
   // 悪い例 - 複数のインスタンス変数を持つクラス
   public class UserCreateCommandHandler {
      // 悪い: インスタンス変数が多すぎる
      private readonly IUserRepository userRepository;
      private readonly IEmailService emailService;
      private readonly ILogger logger;
      private readonly ISmsService smsService;

      public UserCreateCommandHandler(IUserRepository userRepository, IEmailService emailService, ILogger logger, ISmsService smsService) {
         this.userRepository = userRepository;
         this.emailService = emailService;
         this.logger = logger;
         this.smsService = smsService;
      }
   }

   // 良い: 2つのインスタンス変数を持つクラス
   public class UserCreateCommandHandler {
      private readonly IUserRepository userRepository;
      private readonly INotificationService notificationService;
      private readonly ILogger logger; // これはインスタンス変数としてカウントされない

      public UserCreateCommandHandler(IUserRepository userRepository, INotificationService notificationService, ILogger logger) {
         this.userRepository = userRepository;
         this.notificationService = notificationService;
         this.logger = logger;
      }
   }
   ```

9. **ドメインクラスでのゲッター/セッターの禁止**:

   - ドメインクラスでプロパティのセッターを公開することを避ける。
   - オブジェクト作成にはプライベートコンストラクタと静的ファクトリメソッドを使用する。
   - **注意**: このルールは主にドメインクラスに適用され、DTO やデータ転送オブジェクトには適用されない。

   ```csharp
   // 悪い例 - パブリックセッターを持つドメインクラス
   public class User {  // ドメインクラス
       public string Name { get; set; } // ドメインクラスではこれを避ける
   }

   // 良い例 - カプセル化されたドメインクラス
   public class User {  // ドメインクラス
       private string name;
       private User(string name) { this.name = name; }
       public static User Create(string name) => new User(name);
   }

   // 許容される例 - パブリックセッターを持つDTO
   public class UserDto {  // DTO - 適用除外が適用される
       public string Name { get; set; } // DTOでは許容される
   }
   ```

## 実装ガイドライン

- **ドメインクラス**:

  - インスタンス作成にはプライベートコンストラクタと静的ファクトリメソッドを使用する。
  - プロパティのセッターを公開することを避ける。
  - ビジネスドメインコードには 9 つのルールすべてを厳格に適用する。

- **アプリケーション層**:

  - これらのルールをユースケースハンドラーとアプリケーションサービスに適用する。
  - 単一責任とクリーンな抽象化の維持に焦点を当てる。

- **DTO とデータオブジェクト**:

  - ルール 3（プリミティブのラップ）、ルール 8（2 つのインスタンス変数）、ルール 9（ゲッター/セッターなし）は DTO では緩和される場合がある。
  - データ転送オブジェクトではゲッター/セッターを持つパブリックプロパティが許容される。

- **テスト**:

  - テストは状態ではなくオブジェクトの動作を検証することを確実にする。
  - テストクラスは可読性と保守性のためにルールを緩和される場合がある。

- **コードレビュー**:
  - ドメインとアプリケーションコードのコードレビューでこれらのルールを強制する。
  - インフラストラクチャと DTO コードについては実用的に対応する。

## 参考文献

- [Object Calisthenics - Original 9 Rules by Jeff Bay](https://www.cs.helsinki.fi/u/luontola/tdd-2009/ext/ObjectCalisthenics.pdf)
- [ThoughtWorks - Object Calisthenics](https://www.thoughtworks.com/insights/blog/object-calisthenics)
- [Clean Code: A Handbook of Agile Software Craftsmanship - Robert C. Martin](https://www.oreilly.com/library/view/clean-code-a/9780136083238/)
