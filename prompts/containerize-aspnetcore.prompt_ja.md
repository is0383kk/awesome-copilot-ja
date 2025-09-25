---
mode: 'agent'
tools: ['codebase', 'editFiles', 'terminalCommand']
description: 'プロジェクトにカスタマイズされたDockerfileと.dockerfileファイルを作成することで、ASP.NET Coreプロジェクトをコンテナ化する。'
---

# ASP.NET Core Dockerコンテナ化プロンプト

## コンテナ化リクエスト

以下の設定で指定されたASP.NET Core（.NET）プロジェクトをコンテナ化し、**LinuxコンテナでアプリケーションをDockerで実行するために必要な変更のみ**に焦点を当てます。コンテナ化はここで指定されたすべての設定を考慮する必要があります。

.NET Coreアプリケーションをコンテナ化するためのベストプラクティスに準拠し、コンテナがパフォーマンス、セキュリティ、保守性のために最適化されることを確保します。

## コンテナ化設定

このプロンプトのこのセクションには、ASP.NET Coreアプリケーションのコンテナ化に必要な具体的な設定と構成が含まれています。このプロンプトを実行する前に、設定が必要な情報で記入されていることを確認してください。多くの場合、最初の数つの設定のみが必要です。後の設定は、コンテナ化されるプロジェクトに適用されない場合はデフォルトのままにできます。

指定されていない設定はデフォルト値に設定されます。デフォルト値は`[角括弧]`で提供されています。

### 基本プロジェクト情報
1. コンテナ化するプロジェクト: 
   - `[ProjectName (.csprojファイルへのパスを提供)]`

2. 使用する.NETバージョン:
   - `[8.0または9.0（デフォルト8.0）]`

3. 使用するLinuxディストリビューション:
   - `[debian、alpine、ubuntu、chiseled、またはAzure Linux（mariner）（デフォルトdebian）]`

4. Dockerイメージのビルドステージ用のカスタムベースイメージ（標準Microsoftベースイメージを使用する場合は"None"）:
   - `[ビルドステージで使用するベースイメージを指定（デフォルトNone）]`

5. Dockerイメージの実行ステージ用のカスタムベースイメージ（標準Microsoftベースイメージを使用する場合は"None"）:
   - `[実行ステージで使用するベースイメージを指定（デフォルトNone）]`   

### コンテナ設定
1. コンテナイメージで公開する必要があるポート:
   - 主要HTTPポート: `[例: 8080]`
   - 追加ポート: `[追加ポートをリストアップ、または"None"]`

2. コンテナを実行するユーザーアカウント:
   - `[ユーザーアカウント、またはデフォルトで"$APP_UID"]`

3. アプリケーションURL設定:
   - `[ASPNETCORE_URLSを指定、またはデフォルトで"http://+:8080"]`

### ビルド設定
1. コンテナイメージをビルドする前に実行する必要があるカスタムビルドステップ:
   - `[特定のビルドステップをリストアップ、または"None"]`

2. コンテナイメージをビルドした後に実行する必要があるカスタムビルドステップ:
   - `[特定のビルドステップをリストアップ、または"None"]`

3. 設定する必要があるNuGetパッケージソース:
   - `[認証情報を含むプライベートNuGetフィードをリストアップ、または"None"]`

### 依存関係
1. コンテナイメージにインストールする必要があるシステムパッケージ:
   - `[選択されたLinuxディストリビューション用のパッケージ名、または"None"]`

2. コンテナイメージにコピーする必要があるネイティブライブラリ:
   - `[ライブラリ名とパス、または"None"]`

3. インストールする必要がある追加の.NETツール:
   - `[ツール名とバージョン、または"None"]`

### システム設定
1. コンテナイメージに設定する必要がある環境変数:
   - `[変数名と値、または"デフォルトを使用"]`

### ファイルシステム
1. コンテナイメージにコピーする必要があるファイル/ディレクトリ:
   - `[プロジェクトルートからの相対パス、または"None"]`
   - コンテナ内のターゲット場所: `[コンテナパス、または"適用なし"]`

2. コンテナ化から除外するファイル/ディレクトリ:
   - `[除外するパス、または"None"]`

3. 設定すべきボリュームマウントポイント:
   - `[永続化データ用のボリュームパス、または"None"]`

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

- ✅ アプリケーション設定と接続文字列を環境変数から読み取れるようにするアプリ設定変更
- ✅ ASP.NET Coreアプリケーション用のDockerfile作成と設定
- ✅ アプリケーションをビルド/公開し、出力を最終イメージにコピーするためのDockerfileでの複数ステージ指定
- ✅ Linuxコンテナプラットフォーム互換性の設定（Alpine、Ubuntu、Chiseled、またはAzure Linux（Mariner））
- ✅ 依存関係の適切な処理（システムパッケージ、ネイティブライブラリ、追加ツール）
- ❌ インフラストラクチャセットアップなし（別途処理されることを想定）
- ❌ コンテナ化に必要な変更以外のコード変更なし

## 実行プロセス

1. 上記のコンテナ化設定を確認してコンテナ化要件を理解する
2. チェックマーク付きで変更を追跡するための`progress.md`ファイルを作成する
3. プロジェクトの.csprojファイルから`TargetFramework`要素をチェックして.NETバージョンを決定する
4. 以下に基づいて適切なLinuxコンテナイメージを選択する：
   - プロジェクトから検出された.NETバージョン
   - コンテナ化設定で指定されたLinuxディストリビューション（Alpine、Ubuntu、Chiseled、またはAzure Linux（Mariner））
   - ユーザーがコンテナ化設定で特定のベースイメージを要求しない場合、ベースイメージは以下の例のDockerfileに示されているタグ、またはドキュメントに記載されている有効なmcr.microsoft.com/dotnetイメージである必要があります
   - ビルドおよびランタイムステージ用の公式Microsoft .NETイメージ:
      - SDKイメージタグ（ビルドステージ用）: https://github.com/dotnet/dotnet-docker/blob/main/README.sdk.md
      - ASP.NET Coreランタイムイメージタグ: https://github.com/dotnet/dotnet-docker/blob/main/README.aspnet.md
      - .NETランタイムイメージタグ: https://github.com/dotnet/dotnet-docker/blob/main/README.runtime.md
5. アプリケーションをコンテナ化するためのDockerfileをプロジェクトディレクトリのルートに作成する
   - Dockerfileは複数ステージを使用する必要があります：
     - ビルドステージ: .NET SDKイメージを使用してアプリケーションをビルド
       - 最初にcsprojファイルをコピー
       - NuGet.configが存在する場合はコピーし、プライベートフィードを設定
       - NuGetパッケージを復元
       - その後、残りのソースコードをコピーし、アプリケーションをビルド・公開して/app/publishに配置
     - 最終ステージ: 選択された.NETランタイムイメージを使用してアプリケーションを実行
       - 作業ディレクトリを/appに設定
       - 指示された通りにユーザーを設定（デフォルトでは非rootユーザー（例：`$APP_UID`））
         - コンテナ化設定で別途指定されていない限り、新しいユーザーを作成する必要は*ありません*。`$APP_UID`変数を使用してユーザーアカウントを指定してください。
       - ビルドステージからの公開出力を最終イメージにコピー
   - コンテナ化設定のすべての要件を必ず考慮する：
     - .NETバージョンとLinuxディストリビューション
     - 公開ポート
     - コンテナ用ユーザーアカウント
     - ASPNETCORE_URLS設定
     - システムパッケージのインストール
     - ネイティブライブラリ依存関係
     - 追加の.NETツール
     - 環境変数
     - ファイル/ディレクトリのコピー
     - ボリュームマウントポイント
     - ヘルスチェック設定
6. プロジェクトディレクトリのルートに`.dockerignore`ファイルを作成し、Dockerイメージから不要なファイルを除外します。`.dockerignore`ファイルには、コンテナ化設定で指定された追加パターンと併せて、**少なくとも**以下の要素を含める必要があります：
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
7. コンテナ化設定でヘルスチェックが指定されている場合はヘルスチェックを設定する：
   - ヘルスチェックエンドポイントが提供されている場合は、DockerfileにHEALTHCHECK命令を追加
   - curlまたはwgetを使用してヘルスエンドポイントをチェック
8. タスクを完了としてマーク: [ ] → [✓]
9. すべてのタスクが完了し、Dockerビルドが成功するまで継続

## ビルドとランタイム検証

Dockerfileが完成したらDockerビルドが成功することを確認してください。以下のコマンドを使用してDockerイメージをビルドしてください：

```bash
docker build -t aspnetcore-app:latest .
```

ビルドが失敗した場合は、エラーメッセージを確認し、Dockerfileまたはプロジェクト設定に必要な調整を行ってください。成功/失敗を報告してください。

## 進捗追跡

以下の構造で`progress.md`ファイルを維持してください：
```markdown
# コンテナ化進捗

## 環境検出
- [ ] .NETバージョン検出（バージョン: ___）
- [ ] Linuxディストリビューション選択（ディストリビューション: ___）

## 設定変更
- [ ] 環境変数サポート用のアプリケーション設定検証
- [ ] NuGetパッケージソース設定（該当する場合）

## コンテナ化
- [ ] Dockerfile作成
- [ ] .dockerignoreファイル作成
- [ ] SDKイメージでビルドステージ作成
- [ ] パッケージ復元用にcsprojファイルをコピー
- [ ] 該当する場合NuGet.configをコピー
- [ ] ランタイムイメージでランタイムステージ作成
- [ ] 非rootユーザー設定
- [ ] 依存関係処理（システムパッケージ、ネイティブライブラリ、ツール等）
- [ ] ヘルスチェック設定（該当する場合）
- [ ] 特別要件の実装

## 検証
- [ ] コンテナ化設定を確認し、すべての要件が満たされていることを確認
- [ ] Dockerビルド成功
```

ステップ間で確認を求めて一時停止しないでください。アプリケーションがコンテナ化され、Dockerビルドが成功するまで体系的に継続してください。

**すべてのチェックボックスがマークされるまで完了ではありません！** これには、Dockerイメージの正常なビルドとビルドプロセス中に発生する問題への対処が含まれます。

## Dockerfileの例

Linuxベースイメージを使用したASP.NET Core（.NET）アプリケーションのDockerfileの例。

```dockerfile
# ============================================================
# ステージ1: アプリケーションのビルドと公開
# ============================================================

# ベースイメージ - 適切な.NET SDKバージョンとLinuxディストリビューションを選択
# 可能なタグには以下が含まれます：
# - 8.0-bookworm-slim (Debian 12)
# - 8.0-noble (Ubuntu 24.04)
# - 8.0-alpine (Alpine Linux)
# - 9.0-bookworm-slim (Debian 12)
# - 9.0-noble (Ubuntu 24.04)
# - 9.0-alpine (Alpine Linux)
# アプリケーションのビルド用に.NET SDKイメージを使用
FROM mcr.microsoft.com/dotnet/sdk:8.0-bookworm-slim AS build
ARG BUILD_CONFIGURATION=Release

WORKDIR /src

# より良いキャッシングのためにプロジェクトファイルを最初にコピー
COPY ["YourProject/YourProject.csproj", "YourProject/"]
COPY ["YourOtherProject/YourOtherProject.csproj", "YourOtherProject/"]

# 存在する場合はNuGet設定をコピー
COPY ["NuGet.config", "."]

# NuGetパッケージを復元
RUN dotnet restore "YourProject/YourProject.csproj"

# ソースコードをコピー
COPY . .

# 必要に応じて、カスタムプリビルドステップをここで実行
# RUN echo "プリビルドステップを実行中..."

# アプリケーションをビルド
WORKDIR "/src/YourProject"
RUN dotnet build "YourProject.csproj" -c $BUILD_CONFIGURATION -o /app/build

# アプリケーションを公開
RUN dotnet publish "YourProject.csproj" -c $BUILD_CONFIGURATION -o /app/publish /p:UseAppHost=false

# 必要に応じて、カスタムポストビルドステップをここで実行
# RUN echo "ポストビルドステップを実行中..."

# ============================================================
# ステージ2: 最終ランタイムイメージ
# ============================================================

# ベースイメージ - 適切な.NETランタイムバージョンとLinuxディストリビューションを選択
# 可能なタグには以下が含まれます：
# - 8.0-bookworm-slim (Debian 12)
# - 8.0-noble (Ubuntu 24.04)
# - 8.0-alpine (Alpine Linux)
# - 8.0-noble-chiseled (Ubuntu 24.04 Chiseled)
# - 8.0-azurelinux3.0 (Azure Linux)
# - 9.0-bookworm-slim (Debian 12)
# - 9.0-noble (Ubuntu 24.04)
# - 9.0-alpine (Alpine Linux)
# - 9.0-noble-chiseled (Ubuntu 24.04 Chiseled)
# - 9.0-azurelinux3.0 (Azure Linux)
# アプリケーションの実行用に.NETランタイムイメージを使用
FROM mcr.microsoft.com/dotnet/aspnet:8.0-bookworm-slim AS final

# 必要に応じてシステムパッケージをインストール（必要に応じてコメントアウトして変更）
# RUN apt-get update && apt-get install -y \
#     curl \
#     wget \
#     ca-certificates \
#     libgdiplus \
#     && rm -rf /var/lib/apt/lists/*

# 必要に応じて追加の.NETツールをインストール（必要に応じてコメントアウトして変更）
# RUN dotnet tool install --global dotnet-ef --version 8.0.0
# ENV PATH="$PATH:/root/.dotnet/tools"

WORKDIR /app

# ビルドステージから公開されたアプリケーションをコピー
COPY --from=build /app/publish .

# 必要に応じて追加ファイルをコピー（必要に応じてコメントアウトして変更）
# COPY ./config/appsettings.Production.json .
# COPY ./certificates/ ./certificates/

# 環境変数を設定
ENV ASPNETCORE_ENVIRONMENT=Production
ENV ASPNETCORE_URLS=http://+:8080

# 必要に応じてカスタム環境変数を追加（必要に応じてコメントアウトして変更）
# ENV CONNECTIONSTRINGS__DEFAULTCONNECTION="your-connection-string"
# ENV FEATURE_FLAG_ENABLED=true

# 必要に応じてSSL/TLS証明書を設定（必要に応じてコメントアウトして変更）
# ENV ASPNETCORE_Kestrel__Certificates__Default__Path=/app/certificates/app.pfx
# ENV ASPNETCORE_Kestrel__Certificates__Default__Password=your_password

# アプリケーションがリスンするポートを公開
EXPOSE 8080
# EXPOSE 8081  # HTTPSを使用する場合はコメントアウト

# ヘルスチェック用にcurlをインストール（まだ存在しない場合）
RUN apt-get update && apt-get install -y curl && rm -rf /var/lib/apt/lists/*

# ヘルスチェックを設定
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:8080/health || exit 1

# 必要に応じて永続データ用のボリュームを作成（必要に応じてコメントアウトして変更）
# VOLUME ["/app/data", "/app/logs"]

# セキュリティのため非rootユーザーに切り替え
USER $APP_UID

# アプリケーション用のエントリポイントを設定
ENTRYPOINT ["dotnet", "YourProject.dll"]
```

## この例の適用

**注意:** コンテナ化設定の具体的な要件に基づいてこのテンプレートをカスタマイズしてください。

この例のDockerfileを適用する際：

1. `YourProject.csproj`、`YourProject.dll`などを実際のプロジェクト名に置き換える
2. 必要に応じて.NETバージョンとLinuxディストリビューションを調整する
3. 要件に基づいて依存関係インストールステップを変更し、不要なものは削除する
4. アプリケーション固有の環境変数を設定する
5. 特定のワークフローに必要に応じてステージを追加または削除する
6. アプリケーションのヘルスチェックルートに合わせてヘルスチェックエンドポイントを更新する

## Linuxディストリビューションのバリエーション

### Alpine Linux
より小さなイメージサイズを得るためにAlpine Linuxを使用できます：

```dockerfile
FROM mcr.microsoft.com/dotnet/sdk:8.0-alpine AS build
# ... ビルドステップ ...

FROM mcr.microsoft.com/dotnet/aspnet:8.0-alpine AS final
# apkを使用してパッケージをインストール
RUN apk update && apk add --no-cache curl ca-certificates
```

### Ubuntu Chiseled
最小限の攻撃表面を得るためにchiseledイメージを検討してください：

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0-jammy-chiseled AS final
# 注意：Chiseledイメージには最小限のパッケージしか含まれないため、追加の依存関係には異なるベースが必要な場合があります
```

### Azure Linux (Mariner)
Azure最適化コンテナの場合：

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0-azurelinux3.0 AS final
# tdnfを使用してパッケージをインストール
RUN tdnf update -y && tdnf install -y curl ca-certificates && tdnf clean all
```

## ステージ命名に関する注意

- `AS stage-name`構文により各ステージに名前を付けます
- `--from=stage-name`を使用して前のステージからファイルをコピーします
- 最終イメージで使用されない複数の中間ステージを持つことができます
- `final`ステージが最終コンテナイメージになるものです

## セキュリティベストプラクティス

- 本番環境では常に非rootユーザーとして実行する
- `latest`ではなく具体的なイメージタグを使用する
- インストールするパッケージ数を最小限に抑える
- ベースイメージを最新の状態に保つ
- マルチステージビルドを使用して最終イメージからビルド依存関係を除外する