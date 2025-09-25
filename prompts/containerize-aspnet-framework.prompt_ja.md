---
mode: 'agent'
tools: ['codebase', 'editFiles', 'terminalCommand']
description: 'プロジェクトにカスタマイズされたDockerfileと.dockerfileファイルを作成することで、ASP.NET .NET Frameworkプロジェクトをコンテナ化する。'
---

# ASP.NET .NET Framework コンテナ化プロンプト

以下のコンテナ化設定で指定されたASP.NET（.NET Framework）プロジェクトをコンテナ化し、**Windowsコンテナでアプリケーションを実行するために必要な変更のみ**に焦点を当てます。コンテナ化はここで指定されたすべての設定を考慮する必要があります。

**重要:** これは.NET Frameworkアプリケーションであり、.NET Coreではありません。コンテナ化プロセスは.NET Coreアプリケーションとは異なります。

## コンテナ化設定

このプロンプトのこのセクションには、ASP.NET（.NET Framework）アプリケーションのコンテナ化に必要な具体的な設定と構成が含まれています。このプロンプトを実行する前に、設定が必要な情報で記入されていることを確認してください。多くの場合、最初の数つの設定のみが必要です。後の設定は、コンテナ化されるプロジェクトに適用されない場合はデフォルトのままにできます。

指定されていない設定はデフォルト値に設定されます。デフォルト値は`[角括弧]`で提供されています。

### 基本プロジェクト情報
1. コンテナ化するプロジェクト: 
   - `[ProjectName (.csprojファイルへのパスを提供)]`

2. 使用するWindows Server SKU:
   - `[Windows Server Core（デフォルト）またはWindows Server Full]`

3. 使用するWindows Serverバージョン:
   - `[2022、2019、または2016（デフォルト2022）]`

4. Dockerイメージのビルドステージ用のカスタムベースイメージ（標準Microsoftベースイメージを使用する場合は"None"）:
   - `[ビルドステージで使用するベースイメージを指定（デフォルトNone）]`

5. Dockerイメージの実行ステージ用のカスタムベースイメージ（標準Microsoftベースイメージを使用する場合は"None"）:
   - `[実行ステージで使用するベースイメージを指定（デフォルトNone）]`   

### コンテナ設定
1. コンテナイメージで公開する必要があるポート:
   - 主要HTTPポート: `[例: 80]`
   - 追加ポート: `[追加ポートをリストアップ、または"None"]`

2. コンテナを実行するユーザーアカウント:
   - `[ユーザーアカウント、またはデフォルトで"ContainerUser"]`

3. コンテナイメージで設定する必要があるIIS設定:
   - `[特定のIIS設定をリストアップ、または"None"]`

### ビルド設定
1. コンテナイメージをビルドする前に実行する必要があるカスタムビルドステップ:
   - `[特定のビルドステップをリストアップ、または"None"]`

2. コンテナイメージをビルドした後に実行する必要があるカスタムビルドステップ:
   - `[特定のビルドステップをリストアップ、または"None"]`

### 依存関係
1. コンテナイメージのGACに登録すべき.NETアセンブリ:
   - `[アセンブリ名とバージョン、または"None"]`

2. コンテナイメージにコピーしてインストールする必要があるMSI:
   - `[MSI名とバージョン、または"None"]`

3. コンテナイメージに登録する必要があるCOMコンポーネント:
   - `[COMコンポーネント名、または"None"]`

### システム設定
1. コンテナイメージに追加する必要があるレジストリキーと値:
   - `[レジストリパスと値、または"None"]`

2. コンテナイメージに設定する必要がある環境変数:
   - `[変数名と値、または"デフォルトを使用"]`

3. コンテナイメージにインストールする必要があるWindows Serverロールと機能:
   - `[ロール/機能名、または"None"]`

### ファイルシステム
1. コンテナイメージにコピーする必要があるファイル/ディレクトリ:
   - `[プロジェクトルートからの相対パス、または"None"]`
   - コンテナ内のターゲット場所: `[コンテナパス、または"適用なし"]`

2. コンテナ化から除外するファイル/ディレクトリ:
   - `[除外するパス、または"None"]`

### .dockerignore設定
1. `.dockerignore`ファイルに含めるパターン（.dockerignoreには既に一般的なデフォルトが含まれ、これらは追加パターン）:
   - 追加パターン: `[追加パターンをリストアップ、または"None"]`

### ヘルスチェック設定
1. ヘルスチェックエンドポイント:
   - `[ヘルスチェックURLパス、または"None"]`

2. ヘルスチェック間隔とタイムアウト:
   - `[間隔とタイムアウト値、または"デフォルトを使用"]`

### 追加指示
1. プロジェクトをコンテナ化するために従う必要があるその他の指示:
   - `[特定の要件、または"None"]`

2. 対処すべき既知の問題:
   - `[既知の問題を説明、または"None"]`

## 対象範囲

- ✅ アプリ設定の変更により、設定ビルダーを使用してアプリ設定と接続文字列を環境変数から読み取るようにする
- ✅ ASP.NETアプリケーション用のDockerfile作成と設定
- ✅ アプリケーションをビルド/公開し、出力を最終イメージにコピーするためのDockerfileでの複数ステージ指定
- ✅ Windowsコンテナプラットフォーム互換性の設定（Windows Server CoreまたはFull）
- ✅ 依存関係の適切な処理（GACアセンブリ、MSI、COMコンポーネント）
- ❌ インフラストラクチャセットアップなし（別途処理されることを想定）
- ❌ コンテナ化に必要な変更以外のコード変更なし

## 実行プロセス

1. 上記のコンテナ化設定を確認してコンテナ化要件を理解する
2. チェックマーク付きで変更を追跡するための`progress.md`ファイルを作成する
3. プロジェクトの.csprojファイルから`TargetFrameworkVersion`要素をチェックして.NET Frameworkバージョンを決定する
4. 以下に基づいて適切なWindows Serverコンテナイメージを選択する：
   - プロジェクトから検出された.NET Frameworkバージョン
   - コンテナ化設定で指定されたWindows Server SKU（CoreまたはFull）
   - コンテナ化設定で指定されたWindows Serverバージョン（2016、2019、または2022）
   - Windows Server Coreタグは以下で確認可能: https://github.com/microsoft/dotnet-framework-docker/blob/main/README.aspnet.md#full-tag-listing
5. 必要なNuGetパッケージがインストールされていることを確認する。不足している場合は**インストールしない**でください。インストールされていない場合は、ユーザーがVisual Studio NuGetパッケージマネージャーまたはVisual Studioパッケージマネージャーコンソールを使用して手動でインストールする必要があります。インストールされていない場合は、このプロンプトの実行を一時停止し、ユーザーにインストールを求めてください。以下のパッケージが必要です：
   - `Microsoft.Configuration.ConfigurationBuilders.Environment`
6. `web.config`ファイルを変更して設定ビルダーセクションと設定を追加し、環境変数からアプリ設定と接続文字列を読み取るようにする：
   - configSectionsにConfigBuildersセクションを追加
   - ルートにconfigBuildersセクションを追加
   - appSettingsとconnectionStringsの両方にEnvironmentConfigBuilderを設定
   - 例のパターン:
     ```xml
     <configSections>
       <section name="configBuilders" type="System.Configuration.ConfigurationBuildersSection, System.Configuration, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b03f5f7f11d50a3a" restartOnExternalChanges="false" requirePermission="false" />
     </configSections>
     <configBuilders>
       <builders>
         <add name="Environment" type="Microsoft.Configuration.ConfigurationBuilders.EnvironmentConfigBuilder, Microsoft.Configuration.ConfigurationBuilders.Environment" />
       </builders>
     </configBuilders>
     <appSettings configBuilders="Environment">
       <!-- 既存のアプリ設定 -->
     </appSettings>
     <connectionStrings configBuilders="Environment">
       <!-- 既存の接続文字列 -->
     </connectionStrings>
     ```
7. Dockerfileが作成されるフォルダに`LogMonitorConfig.json`ファイルを作成し、このプロンプトの末尾にある参考の`LogMonitorConfig.json`ファイルをコピーする。ファイルの内容は変更せず、コンテナ化設定で別途指定されない限り、参考内容と正確に一致する必要があります。
   - 特に、ログに記録される問題のレベルが変更されないことを確認してください。EventLogソースに`Information`レベルを使用すると不要なノイズが発生します。
8. アプリケーションをコンテナ化するためのDockerfileをプロジェクトディレクトリのルートに作成する
   - Dockerfileは複数ステージを使用する必要があります：
     - ビルドステージ: Windows Server Coreイメージを使用してアプリケーションをビルド
       - ビルドステージは、設定ファイルでカスタムベースイメージが指定されていない限り、`mcr.microsoft.com/dotnet/framework/sdk`ベースイメージを使用する必要があります
       - 最初にsln、csproj、packages.configファイルをコピー
       - NuGet.configが存在する場合はコピーし、プライベートフィードを設定
       - NuGetパッケージを復元       
       - その後、残りのソースコードをコピーし、MSBuildを使用してアプリケーションをビルド・公開してC:\publishに配置
     - 最終ステージ: 選択されたWindows Serverイメージを使用してアプリケーションを実行
       - 最終ステージは、設定ファイルでカスタムベースイメージが指定されていない限り、`mcr.microsoft.com/dotnet/framework/aspnet`ベースイメージを使用する必要があります
       - `LogMonitorConfig.json`ファイルをコンテナ内のディレクトリ（例：C:\LogMonitor）にコピー
       - MicrosoftリポジトリからLogMonitor.exeを同じディレクトリにダウンロード
           - 正しいLogMonitor.exe URLは: https://github.com/microsoft/windows-container-tools/releases/download/v2.1.1/LogMonitor.exe
       - 作業ディレクトリをC:\inetpub\wwwrootに設定
       - ビルドステージからの公開出力（C:\publish内）を最終イメージにコピー
       - コンテナのエントリポイントを設定してLogMonitor.exeでServiceMonitor.exeを実行し、IISサービスを監視
           - `ENTRYPOINT [ "C:\\LogMonitor\\LogMonitor.exe", "C:\\ServiceMonitor.exe", "w3svc" ]`
   - コンテナ化設定のすべての要件を必ず考慮する：
     - Windows Server SKUとバージョン
     - 公開ポート
     - コンテナ用ユーザーアカウント
     - IIS設定
     - GACアセンブリ登録
     - MSIインストール
     - COMコンポーネント登録
     - レジストリキー
     - 環境変数
     - Windowsロールと機能
     - ファイル/ディレクトリのコピー
   - このプロンプトの末尾に提供されている例をモデルにしますが、特定のプロジェクト要件と設定にカスタマイズされていることを確認してください。
   - **重要:** ユーザーが設定ファイルでフルWindows Serverイメージを**具体的に要求**していない限り、Windows Server Coreベースイメージを使用してください
9. プロジェクトディレクトリのルートに`.dockerignore`ファイルを作成し、Dockerイメージから不要なファイルを除外します。`.dockerignore`ファイルには、コンテナ化設定で指定された追加パターンと併せて、**少なくとも**以下の要素を含める必要があります：
   - packages/
   - bin/
   - obj/
   - .dockerignore
   - Dockerfile
   - .git/
   - .github/
   - .vs/
   - .vscode/
   - **/node_modules/
   - *.user
   - *.suo
   - **/.DS_Store
   - **/Thumbs.db
   - コンテナ化設定で指定された追加パターン
10. 設定でヘルスチェックが指定されている場合はヘルスチェックを設定する：
    - ヘルスチェックエンドポイントが提供されている場合は、DockerfileにHEALTHCHECK命令を追加
11. プロジェクトファイルに以下のアイテムを追加してdockerfileをプロジェクトに追加する: `<None Include="Dockerfile" />`
12. タスクを完了としてマーク: [ ] → [✓]
13. すべてのタスクが完了し、Dockerビルドが成功するまで継続

## ビルドとランタイム検証

Dockerfileが完成したらDockerビルドが成功することを確認してください。以下のコマンドを使用してDockerイメージをビルドしてください：

```bash
docker build -t aspnet-app:latest .
```

ビルドが失敗した場合は、エラーメッセージを確認し、Dockerfileまたはプロジェクト設定に必要な調整を行ってください。成功/失敗を報告してください。

## 進捗追跡

以下の構造で`progress.md`ファイルを維持してください：
```markdown
# コンテナ化進捗

## 環境検出
- [ ] .NET Frameworkバージョン検出（バージョン: ___）
- [ ] Windows Server SKU選択（SKU: ___）
- [ ] Windows Serverバージョン選択（バージョン: ___）

## 設定変更
- [ ] 設定ビルダー用のWeb.config変更
- [ ] NuGetパッケージソース設定（該当する場合）
- [ ] LogMonitorConfig.jsonをコピーし、設定で必要に応じて調整

## コンテナ化
- [ ] Dockerfile作成
- [ ] .dockerignoreファイル作成
- [ ] SDKイメージでビルドステージ作成
- [ ] パッケージ復元用にsln、csproj、packages.config、（該当する場合）NuGet.configをコピー
- [ ] ランタイムイメージでランタイムステージ作成
- [ ] 非rootユーザー設定
- [ ] 依存関係処理（GAC、MSI、COM、レジストリ、追加ファイル等）
- [ ] ヘルスチェック設定（該当する場合）
- [ ] 特別要件の実装

## 検証
- [ ] コンテナ化設定を確認し、すべての要件が満たされていることを確認
- [ ] Dockerビルド成功
```

ステップ間で確認を求めて一時停止しないでください。アプリケーションがコンテナ化され、Dockerビルドが成功するまで体系的に継続してください。

**すべてのチェックボックスがマークされるまで完了ではありません！** これには、Dockerイメージの正常なビルドとビルドプロセス中に発生する問題への対処が含まれます。

## 参考資料

### Dockerfileの例

Windows Server Coreベースイメージを使用したASP.NET（.NET Framework）アプリケーションのDockerfileの例。

```dockerfile
# escape=`
# エスケープディレクティブは、エスケープ文字を \ から ` に変更します
# これは特に、\ がパス区切り文字であるWindows Dockerfileで役立ちます

# ============================================================
# ステージ1: アプリケーションのビルドと公開
# ============================================================

# ベースイメージ - 適切な.NET FrameworkバージョンとWindows Server Coreバージョンを選択
# 可能なタグには以下が含まれます：
# - 4.8.1-windowsservercore-ltsc2025 (Windows Server 2025)
# - 4.8-windowsservercore-ltsc2022 (Windows Server 2022)
# - 4.8-windowsservercore-ltsc2019 (Windows Server 2019)
# - 4.8-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7.2-windowsservercore-ltsc2019 (Windows Server 2019)
# - 4.7.2-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7.1-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.6.2-windowsservercore-ltsc2016 (Windows Server 2016)
# - 3.5-windowsservercore-ltsc2025 (Windows Server 2025)
# - 3.5-windowsservercore-ltsc2022 (Windows Server 2022)
# - 3.5-windowsservercore-ltsc2019 (Windows Server 2019)
# - 3.5-windowsservercore-ltsc2019 (Windows Server 2016)
# アプリケーションのビルド用に.NET Framework SDKイメージを使用
FROM mcr.microsoft.com/dotnet/framework/sdk:4.8-windowsservercore-ltsc2022 AS build
ARG BUILD_CONFIGURATION=Release

# デフォルトシェルをPowerShellに設定
SHELL ["powershell", "-command"]

WORKDIR /app

# ソリューションとプロジェクトファイルをコピー
COPY YourSolution.sln .
COPY YourProject/*.csproj ./YourProject/
COPY YourOtherProject/*.csproj ./YourOtherProject/

# packages.configファイルをコピー
COPY YourProject/packages.config ./YourProject/
COPY YourOtherProject/packages.config ./YourOtherProject/

# NuGetパッケージを復元
RUN nuget restore YourSolution.sln

# ソースコードをコピー
COPY . .

# 必要に応じて、カスタムプリビルドステップをここで実行

# アプリケーションをビルドしてC:\publishに公開
RUN msbuild /p:Configuration=$BUILD_CONFIGURATION `
            /p:WebPublishMethod=FileSystem `
            /p:PublishUrl=C:\publish `
            /p:DeployDefaultTarget=WebPublish

# 必要に応じて、カスタムポストビルドステップをここで実行

# ============================================================
# ステージ2: 最終ランタイムイメージ
# ============================================================

# ベースイメージ - 適切な.NET FrameworkバージョンとWindows Server Coreバージョンを選択
# 可能なタグには以下が含まれます：
# - 4.8.1-windowsservercore-ltsc2025 (Windows Server 2025)
# - 4.8-windowsservercore-ltsc2022 (Windows Server 2022)
# - 4.8-windowsservercore-ltsc2019 (Windows Server 2019)
# - 4.8-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7.2-windowsservercore-ltsc2019 (Windows Server 2019)
# - 4.7.2-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7.1-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.7-windowsservercore-ltsc2016 (Windows Server 2016)
# - 4.6.2-windowsservercore-ltsc2016 (Windows Server 2016)
# - 3.5-windowsservercore-ltsc2025 (Windows Server 2025)
# - 3.5-windowsservercore-ltsc2022 (Windows Server 2022)
# - 3.5-windowsservercore-ltsc2019 (Windows Server 2019)
# - 3.5-windowsservercore-ltsc2019 (Windows Server 2016)
# アプリケーションの実行用に.NET Framework ASP.NETイメージを使用
FROM mcr.microsoft.com/dotnet/framework/aspnet:4.8-windowsservercore-ltsc2022

# デフォルトシェルをPowerShellに設定
SHELL ["powershell", "-command"]

WORKDIR /inetpub/wwwroot

# ビルドステージからコピー
COPY --from=build /publish .

# アプリケーションに必要な追加環境変数を追加（必要に応じてコメントアウトして変更）
# ENV KEY=VALUE

# MSIパッケージをインストール（必要に応じてコメントアウトして変更）
# COPY ./msi-installers C:/Installers
# RUN Start-Process -Wait -FilePath 'msiexec.exe' -ArgumentList '/i', 'C:\Installers\your-package.msi', '/quiet', '/norestart'

# カスタムWindows Serverロールと機能をインストール（必要に応じてコメントアウトして変更）
# RUN dism /Online /Enable-Feature /FeatureName:YOUR-FEATURE-NAME

# 追加のWindows機能を追加（必要に応じてコメントアウトして変更）
# RUN Add-WindowsFeature Some-Windows-Feature; `
#    Add-WindowsFeature Another-Windows-Feature

# 必要に応じてMSIパッケージをインストール（必要に応じてコメントアウトして変更）
# COPY ./msi-installers C:/Installers
# RUN Start-Process -Wait -FilePath 'msiexec.exe' -ArgumentList '/i', 'C:\Installers\your-package.msi', '/quiet', '/norestart'

# 必要に応じてアセンブリをGACに登録（必要に応じてコメントアウトして変更）
# COPY ./assemblies C:/Assemblies
# RUN C:\Windows\Microsoft.NET\Framework64\v4.0.30319\gacutil -i C:/Assemblies/YourAssembly.dll

# 必要に応じてCOMコンポーネントを登録（必要に応じてコメントアウトして変更）
# COPY ./com-components C:/Components
# RUN regsvr32 /s C:/Components/YourComponent.dll

# 必要に応じてレジストリキーを追加（必要に応じてコメントアウトして変更）
# RUN New-Item -Path 'HKLM:\Software\YourApp' -Force; `
#     Set-ItemProperty -Path 'HKLM:\Software\YourApp' -Name 'Setting' -Value 'Value'

# 必要に応じてIIS設定を構成（必要に応じてコメントアウトして変更）
# RUN Import-Module WebAdministration; `
#     Set-ItemProperty 'IIS:\AppPools\DefaultAppPool' -Name somePropertyName -Value 'SomePropertyValue'; `
#     Set-ItemProperty 'IIS:\Sites\Default Web Site' -Name anotherPropertyName -Value 'AnotherPropertyValue'

# 必要なポートを公開 - デフォルトでIISはポート80を使用
EXPOSE 80
# EXPOSE 443  # HTTPSを使用する場合はコメントアウト

# microsoft/windows-container-toolsリポジトリからLogMonitorをコピー
WORKDIR /LogMonitor
RUN curl -fSLo LogMonitor.exe https://github.com/microsoft/windows-container-tools/releases/download/v2.1.1/LogMonitor.exe

# ローカルファイルからLogMonitorConfig.jsonをコピー
COPY LogMonitorConfig.json .

# 非管理者ユーザーを設定
USER ContainerUser

# LogMonitorを活用するためにコンテナのデフォルトエントリポイントをオーバーライド
ENTRYPOINT [ "C:\\LogMonitor\\LogMonitor.exe", "C:\\ServiceMonitor.exe", "w3svc" ]
```

## この例の適用

**注意:** コンテナ化設定の具体的な要件に基づいてこのテンプレートをカスタマイズしてください。

この例のDockerfileを適用する際：

1. `YourSolution.sln`、`YourProject.csproj`などを実際のファイル名に置き換える
2. 必要に応じてWindows ServerとNET Frameworkバージョンを調整する
3. 要件に基づいて依存関係インストールステップを変更し、不要なものは削除する
4. 特定のワークフローに必要に応じてステージを追加または削除する

## ステージ命名に関する注意

- `AS stage-name`構文により各ステージに名前を付けます
- `--from=stage-name`を使用して前のステージからファイルをコピーします
- 最終イメージで使用されない複数の中間ステージを持つことができます

### LogMonitorConfig.json

LogMonitorConfig.jsonファイルはプロジェクトディレクトリのルートに作成する必要があります。これはコンテナ内のログを監視するLogMonitorツールの設定に使用されます。このファイルの内容は適切なログ機能を確保するために以下と正確に同じようにする必要があります：
```json
{
  "LogConfig": {
    "sources": [
      {
        "type": "EventLog",
        "startAtOldestRecord": true,
        "eventFormatMultiLine": false,
        "channels": [
          {
            "name": "system",
            "level": "Warning"
          },
          {
            "name": "application",
            "level": "Error"
          }
        ]
      },
      {
        "type": "File",
        "directory": "c:\\inetpub\\logs",
        "filter": "*.log",
        "includeSubdirectories": true,
        "includeFileNames": false
      },
      {
        "type": "ETW",
        "eventFormatMultiLine": false,
        "providers": [
          {
            "providerName": "IIS: WWW Server",
            "providerGuid": "3A2A4E84-4C21-4981-AE10-3FDA0D9B0F83",
            "level": "Information"
          },
          {
            "providerName": "Microsoft-Windows-IIS-Logging",
            "providerGuid": "7E8AD27F-B271-4EA2-A783-A47BDE29143B",
            "level": "Information"
          }
        ]
      }
    ]
  }
}
```