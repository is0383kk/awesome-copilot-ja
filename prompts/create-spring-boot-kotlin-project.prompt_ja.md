---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'findTestFiles', 'problems', 'runCommands', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'testFailure', 'usages']
description: 'Spring Boot Kotlinプロジェクトスケルトンを作成します'
---

# Spring Boot Kotlinプロジェクト作成プロンプト

- システムに以下のソフトウェアがインストールされていることを確認してください：

  - Java 21
  - Docker
  - Docker Compose

- プロジェクト名をカスタマイズする必要がある場合は、[download-spring-boot-project-template](./create-spring-boot-kotlin-project.prompt.md#download-spring-boot-project-template)の`artifactId`と`packageName`を変更してください

- Spring Bootバージョンを更新する必要がある場合は、[download-spring-boot-project-template](./create-spring-boot-kotlin-project.prompt.md#download-spring-boot-project-template)の`bootVersion`を変更してください

## Javaバージョンの確認

- ターミナルで以下のコマンドを実行してJavaのバージョンを確認します

```shell
java -version
```

## Spring Bootプロジェクトテンプレートのダウンロード

- ターミナルで以下のコマンドを実行してSpring Bootプロジェクトテンプレートをダウンロードします

```shell
curl https://start.spring.io/starter.zip \
  -d artifactId=demo \
  -d bootVersion=3.4.5 \
  -d dependencies=configuration-processor,webflux,data-r2dbc,postgresql,data-redis-reactive,data-mongodb-reactive,validation,cache,testcontainers \
  -d javaVersion=21 \
  -d language=kotlin \
  -d packageName=com.example \
  -d packaging=jar \
  -d type=gradle-project-kotlin \
  -o starter.zip
```

## ダウンロードしたファイルの解凍

- ターミナルで以下のコマンドを実行してダウンロードしたファイルを解凍します

```shell
unzip starter.zip -d .
```

## ダウンロードしたzipファイルの削除

- ターミナルで以下のコマンドを実行してダウンロードしたzipファイルを削除します

```shell
rm -f starter.zip
```

## 追加依存関係の追加

- `build.gradle.kts`ファイルに`springdoc-openapi-starter-webmvc-ui`と`archunit-junit5`依存関係を挿入します

```gradle.kts
dependencies {
  implementation("org.springdoc:springdoc-openapi-starter-webflux-ui:2.8.6")
  testImplementation("com.tngtech.archunit:archunit-junit5:1.2.1")
}
```

- `application.properties`ファイルにSpringDoc設定を挿入します

```properties
# SpringDoc configurations
springdoc.swagger-ui.doc-expansion=none
springdoc.swagger-ui.operations-sorter=alpha
springdoc.swagger-ui.tags-sorter=alpha
```

- `application.properties`ファイルにRedis設定を挿入します

```properties
# Redis configurations
spring.data.redis.host=localhost
spring.data.redis.port=6379
spring.data.redis.password=rootroot
```

- `application.properties`ファイルにR2DBC設定を挿入します

```properties
# R2DBC configurations
spring.r2dbc.url=r2dbc:postgresql://localhost:5432/postgres
spring.r2dbc.username=postgres
spring.r2dbc.password=rootroot

spring.sql.init.mode=always
spring.sql.init.platform=postgres
spring.sql.init.continue-on-error=true
```

- `application.properties`ファイルにMongoDB設定を挿入します

```properties
# MongoDB configurations
spring.data.mongodb.host=localhost
spring.data.mongodb.port=27017
spring.data.mongodb.authentication-database=admin
spring.data.mongodb.username=root
spring.data.mongodb.password=rootroot
spring.data.mongodb.database=test
```

- プロジェクトルートに`docker-compose.yaml`を作成し、以下のサービスを追加します：`redis:6`、`postgresql:17`、`mongo:8`。

  - redisサービスには以下を設定します
    - パスワード `rootroot`
    - ポート6379を6379にマッピング
    - ボリューム `./redis_data` を `/data` にマウント
  - postgresqlサービスには以下を設定します
    - パスワード `rootroot`
    - ポート5432を5432にマッピング
    - ボリューム `./postgres_data` を `/var/lib/postgresql/data` にマウント
  - mongoサービスには以下を設定します
    - initdbルートユーザー名 `root`
    - initdbルートパスワード `rootroot`
    - ポート27017を27017にマッピング
    - ボリューム `./mongo_data` を `/data/db` にマウント

- `.gitignore`ファイルに`redis_data`、`postgres_data`、`mongo_data`ディレクトリを挿入します

- プロジェクトが動作するかを確認するためにgradle clean testコマンドを実行します

```shell
./gradlew clean test
```

- （オプション）`docker-compose up -d`でサービスを開始し、`./gradlew spring-boot:run`でSpring Bootプロジェクトを実行し、`docker-compose rm -sf`でサービスを停止します。

これをステップバイステップで進めましょう。