---
description: '.NET WPFコンポーネントとアプリケーションパターン'
applyTo: '**/*.xaml, **/*.cs'
---

## 概要

これらの指針は、MVVMパターンを使用した高品質で保守性があり、パフォーマンスの高いWPFアプリケーションの構築において、GitHub Copilotがサポートするためのものです。XAML、データバインディング、UI応答性、.NETパフォーマンスのベストプラクティスが含まれています。

## 理想的なプロジェクトタイプ

- C#とWPFを使用したデスクトップアプリケーション
- MVVM（Model-View-ViewModel）デザインパターンに従うアプリケーション
- .NET 8.0以降を使用するプロジェクト
- XAMLで構築されたUIコンポーネント
- パフォーマンスと応答性を重視するソリューション

## 目標

- `INotifyPropertyChanged`と`RelayCommand`のボイラープレートを生成
- ViewModelとViewロジックの明確な分離を提案
- `ObservableCollection<T>`、`ICommand`、適切なバインディングの使用を推奨
- パフォーマンスのヒント（仮想化、非同期読み込みなど）を推奨
- コードビハインドロジックの密結合を避ける
- テスト可能なViewModelを生成

## プロンプトの例の動作

### ✅ 良い提案
- "ユーザー名とパスワードのプロパティとLoginCommandを持つログイン画面のViewModelを生成する"
- "UI仮想化を使用し、ObservableCollectionにバインドするListViewのXAMLスニペットを作成する"
- "このコードビハインドのクリックハンドラーをViewModelのRelayCommandにリファクタリングする"
- "WPFでデータを非同期取得中にローディングスピナーを追加する"

### ❌ 避けるべき
- コードビハインドでのビジネスロジックを提案
- コンテキストなしの静的イベントハンドラーの使用
- バインディングなしの密結合したXAMLの生成
- WinFormsやUWPアプローチの提案

## 推奨技術
- .NET 8.0+を使用したC#
- MVVM構造を持つXAML
- `CommunityToolkit.Mvvm`またはカスタム`RelayCommand`実装
- ノンブロッキングUIのためのAsync/await
- `ObservableCollection`、`ICommand`、`INotifyPropertyChanged`

## 従うべき一般的なパターン
- ViewModel-firstバインディング
- .NETまたはサードパーティコンテナ（Autofac、SimpleInjectorなど）を使用した依存性注入
- XAML命名規則（コントロールにはPascalCase、バインディングにはcamelCase）
- バインディングでのマジック文字列を避ける（`nameof`を使用）

## Copilotが使用できるサンプル指針スニペット

```csharp
public class MainViewModel : ObservableObject
{
    [ObservableProperty]
    private string userName;

    [ObservableProperty]
    private string password;

    [RelayCommand]
    private void Login()
    {
        // ここにログインロジックを追加
    }
}
```

```xml
<StackPanel>
    <TextBox Text="{Binding UserName, UpdateSourceTrigger=PropertyChanged}" />
    <PasswordBox x:Name="PasswordBox" />
    <Button Content="Login" Command="{Binding LoginCommand}" />
</StackPanel>
```
