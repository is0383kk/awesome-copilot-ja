---
description: "Microsoft Power Apps YAML スキーマ v3.0 に基づいた Power Apps Canvas Apps の YAML 構造の包括的なガイド。Power Fx 数式、コントロール構造、データ型、ソースコントロールのベストプラクティスをカバー。"
applyTo: "**/*.{yaml,yml,md,pa.yaml}"
---

# Power Apps Canvas Apps YAML 構造ガイド

## 概要

このドキュメントは、公式の Microsoft Power Apps YAML スキーマ（v3.0）と Power Fx ドキュメントに基づいて、Power Apps Canvas Apps の YAML コードを扱うための包括的な指示を提供します。

**公式スキーマソース**: https://raw.githubusercontent.com/microsoft/PowerApps-Tooling/refs/heads/master/schemas/pa-yaml/v3.0/pa.schema.yaml

## Power Fx 設計原則

Power Fx は Power Apps Canvas Apps 全体で使用される数式言語です。以下の中核原則に従います：

### 設計原則

- **シンプル**: Excel の数式に馴染みのある概念を使用
- **Excel との一貫性**: Excel の数式構文と動作に合致
- **宣言的**: 方法ではなく、何を望むかを記述
- **関数型**: 副作用を避ける；ほとんどの関数は純粋
- **構成**: より単純な関数を組み合わせて複雑なロジックを構築
- **強い型付け**: 型システムがデータの整合性を保証
- **統合**: Power Platform 全体でシームレスに動作

### 言語哲学

Power Fx が促進するもの：

- Excel のような馴染みある数式による ローコード開発
- 依存関係の変更時の自動再計算
- コンパイル時チェックによる型安全性
- 関数プログラミングパターン

## ルート構造

すべての Power Apps YAML ファイルは、この最上位構造に従います：

```yaml
App:
  Properties:
    # アプリレベルプロパティと数式
    StartScreen: =Screen1

Screens:
  # スクリーン定義

ComponentDefinitions:
  # カスタムコンポーネント定義

DataSources:
  # データソース構成

EditorState:
  # エディタメタデータ（スクリーン順序など）
```

## 1. App セクション

`App` セクションはアプリケーションレベルのプロパティと構成を定義します。

```yaml
App:
  Properties:
    StartScreen: =Screen1
    BackEnabled: =false
    # Power Fx 数式を使用したその他のアプリプロパティ
```

### キーポイント：

- アプリケーション全体の設定を含む
- プロパティは Power Fx 数式を使用（`=` プレフィックス）
- `StartScreen` プロパティが一般的に指定される

## 2. Screens セクション

アプリケーション内のすべてのスクリーンを順序なしマップとして定義します。

```yaml
Screens:
  Screen1:
    Properties:
      # スクリーンプロパティ
    Children:
      - Label1:
          Control: Label
          Properties:
            Text: ="Hello World"
            X: =10
            Y: =10
      - Button1:
          Control: Button
          Properties:
            Text: ="Click Me"
            X: =10
            Y: =100
```

### スクリーン構造：

- **Properties**: スクリーンレベルのプロパティと数式
- **Children**: スクリーン上のコントロールの配列（z-index 順）

### コントロール定義形式：

```yaml
ControlName:
  Control: ControlType # 必須: コントロール型識別子
  Properties:
    PropertyName: =PowerFxFormula
  # オプションプロパティ:
  Group: GroupName # Studio でのコントロール整理用
  Variant: VariantName # コントロールバリアント（デフォルトプロパティに影響）
  MetadataKey: Key # コントロールのメタデータ識別子
  Layout: LayoutName # レイアウト構成
  IsLocked: true/false # エディタでコントロールがロックされているか
  Children: [] # コンテナコントロール用（z-index順）
```

### コントロールバージョニング：

`@` 演算子を使用してコントロールバージョンを指定できます：

```yaml
MyButton:
  Control: Button@2.1.0 # 特定のバージョン
  Properties:
    Text: ="Click Me"

MyLabel:
  Control: Label # デフォルトで最新バージョンを使用
  Properties:
    Text: ="Hello World"
```

## 3. コントロール型

### 標準コントロール

一般的な第一者コントロールには以下があります：

- **基本コントロール**: `Label`, `Button`, `TextInput`, `HTMLText`
- **入力コントロール**: `Slider`, `Toggle`, `Checkbox`, `Radio`, `Dropdown`, `Combobox`, `DatePicker`, `ListBox`
- **表示コントロール**: `Image`, `Icon`, `Video`, `Audio`, `PDF viewer`, `Barcode scanner`
- **レイアウトコントロール**: `Container`, `Rectangle`, `Circle`, `Gallery`, `DataTable`, `Form`
- **チャートコントロール**: `Column chart`, `Line chart`, `Pie chart`
- **高度なコントロール**: `Timer`, `Camera`, `Microphone`, `Add picture`, `Import`, `Export`

### コンテナとレイアウトコントロール

コンテナコントロールとその子要素への特別な注意：

```yaml
MyContainer:
  Control: Container
  Properties:
    Width: =300
    Height: =200
    Fill: =RGBA(240, 240, 240, 1)
  Children:
    - Label1:
        Control: Label
        Properties:
          Text: ="Inside Container"
          X: =10 # コンテナに相対
          Y: =10 # コンテナに相対
    - Button1:
        Control: Button
        Properties:
          Text: ="Container Button"
          X: =10
          Y: =50
```

### カスタムコンポーネント

```yaml
MyCustomControl:
  Control: Component
  ComponentName: MyComponent
  Properties:
    X: =10
    Y: =10
    # カスタムコンポーネントプロパティ
```

### コードコンポーネント（PCF）

```yaml
MyPCFControl:
  Control: CodeComponent
  ComponentName: publisherprefix_namespace.classname
  Properties:
    X: =10
    Y: =10
```

## 4. Component Definitions

再利用可能なカスタムコンポーネントを定義：

```yaml
ComponentDefinitions:
  MyComponent:
    DefinitionType: CanvasComponent
    Description: "A reusable component"
    AllowCustomization: true
    AccessAppScope: false
    CustomProperties:
      InputText:
        PropertyKind: Input
        DataType: Text
        Description: "Input text property"
        Default: ="Default Value"
      OutputValue:
        PropertyKind: Output
        DataType: Number
        Description: "Output number value"
    Properties:
      Fill: =RGBA(255, 255, 255, 1)
      Height: =100
      Width: =200
    Children:
      - Label1:
          Control: Label
          Properties:
            Text: =Parent.InputText
```

### カスタムプロパティ型：

- **Input**: 親から値を受信
- **Output**: 親に値を送信
- **InputFunction**: 親によって呼び出される関数
- **OutputFunction**: コンポーネントで定義される関数
- **Event**: 親にイベントをトリガー
- **Action**: 副作用を持つ関数

### データ型：

- `Text`, `Number`, `Boolean`
- `DateAndTime`, `Color`, `Currency`
- `Record`, `Table`, `Image`
- `VideoOrAudio`, `Screen`

## 5. Data Sources

データ接続を構成：

```yaml
DataSources:
  MyTable:
    Type: Table
    Parameters:
      TableLogicalName: account

  MyActions:
    Type: Actions
    ConnectorId: shared_office365users
    Parameters:
      # 追加のコネクターパラメーター
```

### データソース型：

- **Table**: Dataverse テーブルやその他の表形式データ
- **Actions**: コネクターアクションとフロー

## 6. Editor State

エディターの構成を維持：

```yaml
EditorState:
  ScreensOrder:
    - Screen1
    - Screen2
    - Screen3
  ComponentDefinitionsOrder:
    - MyComponent
    - AnotherComponent
```

## Power Fx 数式ガイドライン

### 数式構文：

- すべての数式は `=` で開始する必要があります
- 式には Power Fx 構文を使用
- null 値は `null`（引用符なし）として表現できます
- 例：
  ```yaml
  Text: ="Hello World"
  X: =10
  Visible: =Toggle1.Value
  OnSelect: =Navigate(Screen2, ScreenTransition.Fade)
  OptionalProperty: null # 値なしを表す
  ```

### 一般的な数式パターン：

```yaml
# 静的値
Text: ="Static Text"
X: =50
Visible: =true

# コントロール参照
Text: =TextInput1.Text
Visible: =Toggle1.Value

# 親参照（コンテナ/ギャラリー内のコントロール用）
Width: =Parent.Width - 20
Height: =Parent.TemplateHeight    # ギャラリーテンプレート内

# 関数
OnSelect: =Navigate(NextScreen, ScreenTransition.Slide)
Text: =Concatenate("Hello ", User().FullName)

# 条件ロジック
Visible: =If(Toggle1.Value, true, false)
Fill: =If(Button1.Pressed, RGBA(255,0,0,1), RGBA(0,255,0,1))

# データ操作
Items: =Filter(DataSource, Status = "Active")
Text: =LookUp(Users, ID = 123).Name
```

### Z-Index とコントロール順序：

- `Children` 配列内のコントロールは z-index 順に並べられます
- 配列の最初のコントロール = 最下位レイヤー（z-index 1）
- 配列の最後のコントロール = 最上位レイヤー（最高 z-index）
- すべてのコントロールは 1 から始まる昇順を使用

## 命名規約

### エンティティ名：

- スクリーン名: 説明的で一意
- コントロール名: TypeName + Number（例：`Button1`, `Label2`）
- コンポーネント名: PascalCase

### プロパティ名：

- 標準プロパティ: スキーマの正確な大文字小文字を使用
- カスタムプロパティ: PascalCase 推奨

## ベストプラクティス

### 1. 構造の組織化：

- スクリーンを論理的に整理する
- `Group` プロパティを使用して関連するコントロールをグループ化する
- すべてのエンティティに意味のある名前を使用する

### 2. 数式の記述：

- 数式を読みやすく、適切にフォーマットする
- 可能な場合、複雑な数式にコメントを使用する
- 過度に複雑な入れ子式を避ける

### 3. コンポーネント設計：

- 再利用可能なコンポーネントを設計する
- カスタムプロパティに明確な説明を提供する
- 適切なプロパティ種別（Input/Output）を使用する

### 4. データソース管理：

- データソースには説明的な名前を使用する
- 接続要件を文書化する
- データソース構成を最小限に保つ

## 検証ルール

### 必須プロパティ：

- すべてのコントロールは `Control` プロパティを持つ必要があります
- コンポーネント定義は `DefinitionType` を持つ必要があります
- データソースは `Type` を持つ必要があります

### 命名パターン：

- エンティティ名: 最小 1 文字、英数字
- コントロール型 ID: パターン `^([A-Z][a-zA-Z0-9]*/)?[A-Z][a-zA-Z0-9]*(@\d+\.\d+\.\d+)?$` に従う
- コードコンポーネント名: パターン `^([a-z][a-z0-9]{1,7})_([a-zA-Z0-9]\.)+[a-zA-Z0-9]+$` に従う

## 一般的な問題と解決策

### 1. 無効なコントロール型：

- コントロール型のスペルが正しいことを確認
- 適切な大文字小文字を確認
- コントロール型がスキーマでサポートされていることを確認

### 2. 数式エラー：

- すべての数式は `=` で始まる必要があります
- 適切な Power Fx 構文を使用
- 正しいプロパティ参照を確認

### 3. 構造検証：

- 適切な YAML インデンテーションを維持
- 必須プロパティが存在することを確認
- スキーマ構造に正確に従う

### 4. カスタムコンポーネントの問題：

- `ComponentName` が定義と一致することを確認
- カスタムプロパティが適切に定義されていることを確認
- プロパティ種別が適切であることを確認
- 外部コンポーネントを使用する場合、コンポーネントライブラリ参照を検証

### 5. パフォーマンスの考慮事項：

- YAML 内で深い入れ子の数式を避ける
- 効率的なデータソースクエリを使用
- 大きなデータセットには委任可能な数式を検討
- 頻繁に更新されるプロパティでの複雑な計算を最小化

## 高度なトピック

### 1. コンポーネントライブラリ統合：

```yaml
ComponentDefinitions:
  MyLibraryComponent:
    DefinitionType: CanvasComponent
    AllowCustomization: true
    ComponentLibraryUniqueName: "pub_MyComponentLibrary"
    # コンポーネント定義詳細
```

### 2. レスポンシブ設計の考慮事項：

- レスポンシブサイズ設定には `Parent.Width` と `Parent.Height` を使用
- 複雑な UI にはコンテナベースのレイアウトを検討
- 動的な配置とサイズ設定には数式を使用

### 3. ギャラリーテンプレート：

```yaml
MyGallery:
  Control: Gallery
  Properties:
    Items: =DataSource
    TemplateSize: =100
  Children:
    - GalleryTemplate: # 各ギャラリー項目のテンプレート
        Children:
          - TitleLabel:
              Control: Label
              Properties:
                Text: =ThisItem.Title
                Width: =Parent.TemplateWidth - 20
```

### 4. フォームコントロールとデータカード：

```yaml
MyForm:
  Control: Form
  Properties:
    DataSource: =DataSource
    DefaultMode: =FormMode.New
  Children:
    - DataCard1:
        Control: DataCard
        Properties:
          DataField: ="Title"
        Children:
          - DataCardValue1:
              Control: TextInput
              Properties:
                Default: =Parent.Default
```

### 5. 数式でのエラーハンドリング：

```yaml
Properties:
  Text: =IfError(LookUp(DataSource, ID = 123).Name, "Not Found")
  Visible: =!IsError(DataSource)
  OnSelect: =IfError(
    Navigate(DetailScreen, ScreenTransition.Cover),
    Notify("Navigation failed", NotificationType.Error)
  )
```

## Power Apps ソースコード管理

### ソースコードファイルへのアクセス：

Power Apps YAML ファイルは複数の方法で取得できます：

1. **Power Platform CLI**:

   ```powershell
   # 環境内のキャンバスアプリをリスト表示
   pac canvas list

   # YAML ファイルをダウンロードして展開
   pac canvas download --name "MyApp" --extract-to-directory "C:\path\to\destination"
   ```

2. **.msapp からの手動展開**:

   ```powershell
   # PowerShell を使用して .msapp ファイルを展開
   Expand-Archive -Path "C:\path\to\yourFile.msapp" -DestinationPath "C:\path\to\destination"
   ```

3. **Dataverse Git 統合**: .msapp ファイルなしでソースファイルに直接アクセス

### .msapp 内のファイル構造：

- `\src\App.pa.yaml` - メインアプリ構成を表す
- `\src\[ScreenName].pa.yaml` - 各スクリーンに 1 つのファイル
- `\src\Component\[ComponentName].pa.yaml` - コンポーネント定義

**重要な注意事項**:

- `\src` フォルダ内のファイルのみがソースコントロール対象
- .pa.yaml ファイルは**読み取り専用**でレビュー目的のみ
- 外部編集、マージ、競合解決はサポートされていません
- .msapp 内の JSON ファイルはソースコントロールに対して安定していません

### スキーマバージョンの進化：

1. **実験的形式** (\*.fx.yaml): 開発中止
2. **初期プレビュー**: 一時的形式、使用中止
3. **ソースコード** (\*.pa.yaml): バージョンコントロールサポートを含む現在のアクティブ形式

## Power Fx 数式リファレンス

### 数式カテゴリ：

#### **関数**: パラメーターを取り、操作を実行し、値を返す

```yaml
Properties:
  Text: =Concatenate("Hello ", User().FullName)
  X: =Sum(10, 20, 30)
  Items: =Filter(DataSource, Status = "Active")
```

#### **シグナル**: 環境情報を返す（パラメーターなし）

```yaml
Properties:
  Text: =Location.Latitude & ", " & Location.Longitude
  Visible: =Connection.Connected
  Color: =If(Acceleration.X > 5, Color.Red, Color.Blue)
```

#### **列挙**: 事前定義された定数値

```yaml
Properties:
  Fill: =Color.Blue
  Transition: =ScreenTransition.Fade
  Align: =Align.Center
```

#### **名前付き演算子**: コンテナ情報にアクセス

```yaml
Properties:
  Text: =ThisItem.Title # ギャラリー内
  Width: =Parent.Width - 20 # コンテナ内
  Height: =Self.Height / 2 # 自己参照
```

### YAML 用の必須 Power Fx 関数：

#### **ナビゲーションとアプリ制御**:

```yaml
OnSelect: =Navigate(NextScreen, ScreenTransition.Cover)
OnSelect: =Back()
OnSelect: =Exit()
OnSelect: =Launch("https://example.com")
```

#### **データ操作**:

```yaml
Items: =Filter(DataSource, Category = "Active")
Text: =LookUp(Users, ID = 123).Name
OnSelect: =Patch(DataSource, ThisItem, {Status: "Complete"})
OnSelect: =Collect(LocalCollection, {Name: TextInput1.Text})
```

#### **条件ロジック**:

```yaml
Visible: =If(Toggle1.Value, true, false)
Text: =Switch(Status, "New", "🆕", "Complete", "✅", "❓")
Fill: =If(Value < 0, Color.Red, Color.Green)
```

#### **テキスト操作**:

```yaml
Text: =Concatenate("Hello ", User().FullName)
Text: =Upper(TextInput1.Text)
Text: =Substitute(Label1.Text, "old", "new")
Text: =Left(Title, 10) & "..."
```

#### **数学演算**:

```yaml
Text: =Sum(Sales[Amount])
Text: =Average(Ratings[Score])
Text: =Round(Calculation, 2)
Text: =Max(Values[Number])
```

#### **日時関数**:

```yaml
Text: =Text(Now(), "mm/dd/yyyy")
Text: =DateDiff(StartDate, EndDate, Days)
Text: =Text(Today(), "dddd, mmmm dd, yyyy")
Visible: =IsToday(DueDate)
```

### 数式構文ガイドライン：

#### **基本構文ルール**:

- すべての数式は `=` で始まる
- 先頭に `+` や `=` 記号なし（Excel と異なる）
- テキスト文字列は二重引用符: `="Hello World"`
- プロパティ参照: `ControlName.PropertyName`
- YAML コンテキストではコメントはサポートされていません

#### **数式要素**:

```yaml
# リテラル値
Text: ="Static Text"
X: =42
Visible: =true

# コントロールプロパティ参照
Text: =TextInput1.Text
Visible: =Checkbox1.Value

# 関数呼び出し
Text: =Upper(TextInput1.Text)
Items: =Sort(DataSource, Title)

# 複合式
Text: =If(IsBlank(TextInput1.Text), "Enter text", Upper(TextInput1.Text))
```

#### **動作対プロパティ数式**:

```yaml
# プロパティ数式（値を計算）
Properties:
  Text: =Concatenate("Hello ", User().FullName)
  Visible: =Toggle1.Value

# 動作数式（アクションを実行 - 複数アクションにはセミコロンを使用）
Properties:
  OnSelect: =Set(MyVar, true); Navigate(NextScreen); Notify("Done!")
```

### 高度な数式パターン：

#### **コレクションの操作**:

```yaml
Properties:
  Items: =Filter(MyCollection, Status = "Active")
  OnSelect: =ClearCollect(MyCollection, DataSource)
  OnSelect: =Collect(MyCollection, {Name: "New Item", Status: "Active"})
```

#### **エラーハンドリング**:

```yaml
Properties:
  Text: =IfError(Value(TextInput1.Text), 0)
  OnSelect: =IfError(
    Patch(DataSource, ThisItem, {Field: Value}),
    Notify("Error updating record", NotificationType.Error)
  )
```

#### **動的プロパティ設定**:

```yaml
Properties:
  Fill: =ColorValue("#" & HexInput.Text)
  Height: =Parent.Height * (Slider1.Value / 100)
  X: =If(Alignment = "Center", (Parent.Width - Self.Width) / 2, 0)
```

## 数式を使用するベストプラクティス

### 数式の組織化：

- 複雑な数式をより小さく読みやすい部分に分割
- 中間計算を格納するために変数を使用
- 説明的なコントロール名を使用して複雑なロジックにコメント
- 関連する計算をまとめてグループ化

### パフォーマンス最適化：

- 大きなデータセットを扱う場合は委任フレンドリーな関数を使用
- 頻繁に更新されるプロパティでの入れ子関数呼び出しを避ける
- 複雑なデータ変換にはコレクションを使用
- 外部データソースへの呼び出しを最小化

## Power Fx データ型と操作

### データ型カテゴリ：

#### **プリミティブ型**:

- **Boolean**: `=true`, `=false`
- **Number**: `=123`, `=45.67`
- **Text**: `="Hello World"`
- **Date**: `=Date(2024, 12, 25)`
- **Time**: `=Time(14, 30, 0)`
- **DateTime**: `=Now()`

#### **複合型**:

- **Color**: `=Color.Red`, `=RGBA(255, 128, 0, 1)`
- **Record**: `={Name: "John", Age: 30}`
- **Table**: `=Table({Name: "John"}, {Name: "Jane"})`
- **GUID**: `=GUID()`

#### **型変換**:

```yaml
Properties:
  Text: =Text(123.45, "#,##0.00")        # 数値をテキストに
  Text: =Value("123.45")                 # テキストを数値に
  Text: =DateValue("12/25/2024")         # テキストを日付に
  Visible: =Boolean("true")              # テキストをブール値に
```

#### **型チェック**:

```yaml
Properties:
  Visible: =Not(IsBlank(OptionalField))
  Visible: =Not(IsError(Value(TextInput1.Text)))
  Visible: =IsNumeric(TextInput1.Text)
```

### テーブル操作：

#### **テーブル作成**:

```yaml
Properties:
  Items: =Table(
    {Name: "Product A", Price: 10.99},
    {Name: "Product B", Price: 15.99}
  )
  Items: =["Option 1", "Option 2", "Option 3"]  # 単一列テーブル
```

#### **フィルタリングとソート**:

```yaml
Properties:
  Items: =Filter(Products, Price > 10)
  Items: =Sort(Products, Name, Ascending)
  Items: =SortByColumns(Products, "Price", Descending, "Name", Ascending)
```

#### **データ変換**:

```yaml
Properties:
  Items: =AddColumns(Products, "Total", Price * Quantity)
  Items: =RenameColumns(Products, "Price", "Cost")
  Items: =ShowColumns(Products, "Name", "Price")
  Items: =DropColumns(Products, "InternalID")
```

#### **集計**:

```yaml
Properties:
  Text: =Sum(Products, Price)
  Text: =Average(Products, Rating)
  Text: =Max(Products, Price)
  Text: =CountRows(Products)
```

### 変数と状態管理：

#### **グローバル変数**:

```yaml
Properties:
  OnSelect: =Set(MyGlobalVar, "Hello World")
  Text: =MyGlobalVar
```

#### **コンテキスト変数**:

```yaml
Properties:
  OnSelect: =UpdateContext({LocalVar: "Screen Specific"})
  OnSelect: =Navigate(NextScreen, None, {PassedValue: 42})
```

#### **コレクション**:

```yaml
Properties:
  OnSelect: =ClearCollect(MyCollection, DataSource)
  OnSelect: =Collect(MyCollection, {Name: "New Item"})
  Items: =MyCollection
```

## Power Fx 拡張コネクタと外部データ

### コネクタ統合：

```yaml
DataSources:
  SharePointList:
    Type: Table
    Parameters:
      TableLogicalName: "Custom List"

  Office365Users:
    Type: Actions
    ConnectorId: shared_office365users
```

### 外部データの操作：

```yaml
Properties:
  Items: =Filter(SharePointList, Status = "Active")
  OnSelect: =Office365Users.SearchUser({searchTerm: SearchInput.Text})
```

### 委任の考慮事項：

```yaml
Properties:
  # 委任可能な操作（サーバー側で実行）
  Items: =Filter(LargeTable, Status = "Active")    # 効率的

  # 委任不可能な操作（すべてのレコードをダウンロードする可能性）
  Items: =Filter(LargeTable, Len(Description) > 100)  # 警告が発行される
```

## トラブルシューティングと一般的なパターン

### 一般的なエラーパターン：

```yaml
# 空白値の処理
Properties:
  Text: =If(IsBlank(OptionalText), "Default", OptionalText)

# エラーを適切に処理
Properties:
  Text: =IfError(RiskyOperation(), "Fallback Value")

# 入力の検証
Properties:
  Visible: =And(
    Not(IsBlank(NameInput.Text)),
    IsNumeric(AgeInput.Text),
    IsMatch(EmailInput.Text, Email)
  )
```

### パフォーマンス最適化：

```yaml
# 効率的なデータ読み込み
Properties:
  Items: =Filter(LargeDataSource, Status = "Active")    # サーバー側フィルタリング

# 委任フレンドリーな操作を使用
Properties:
  Items: =Sort(Filter(DataSource, Active), Name)        # 委任可能
  # 避ける: Sort(DataSource, If(Active, Name, ""))       # 委任不可能
```

### メモリ管理：

```yaml
# 未使用のコレクションをクリア
Properties:
  OnSelect: =Clear(TempCollection)

# データ取得を制限
Properties:
  Items: =FirstN(Filter(DataSource, Status = "Active"), 50)
```

覚えておいてください：このガイドは、Power Apps Canvas Apps YAML 構造と Power Fx 数式の包括的なカバレッジを提供します。常に YAML を公式スキーマに対して検証し、Power Apps Studio 環境で数式をテストしてください。
