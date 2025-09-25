---
mode: 'agent'
tools: ['changes', 'codebase', 'editFiles', 'findTestFiles', 'problems', 'runCommands', 'runTests', 'search', 'searchResults', 'terminalLastCommand', 'testFailure', 'usages']
description: 'Spring Boot Javaプロジェクトスケルトンを作成します'
---

# Spring Boot Javaプロジェクト作成プロンプト

- システムに以下のソフトウェアがインストールされていることを確認してください：

  - Java 21
  - Docker
  - Docker Compose

- プロジェクト名をカスタマイズする必要がある場合は、[download-spring-boot-project-template](./create-spring-boot-java-project.prompt.md#download-spring-boot-project-template)の`artifactId`と`packageName`を変更してください

- Spring Bootバージョンを更新する必要がある場合は、[download-spring-boot-project-template](./create-spring-boot-java-project.prompt.md#download-spring-boot-project-template)の`bootVersion`を変更してください

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
  -d dependencies=lombok,configuration-processor,web,data-jpa,postgresql,data-redis,data-mongodb,validation,cache,testcontainers \
  -d javaVersion=21 \
  -d packageName=com.example \
  -d packaging=jar \
  -d type=maven-project \
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

- `pom.xml`ファイルに`springdoc-openapi-starter-webmvc-ui`と`archunit-junit5`依存関係を挿入します

```xml
<dependency>
  <groupId>org.springdoc</groupId>
  <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
  <version>2.8.6</version>
</dependency>
<dependency>
  <groupId>com.tngtech.archunit</groupId>
  <artifactId>archunit-junit5</artifactId>
  <version>1.2.1</version>
  <scope>test</scope>
</dependency>
```

## SpringDoc、Redis、JPA、MongoDB設定の追加

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

- `application.properties`ファイルにJPA設定を挿入します

```properties
# JPA configurations
spring.datasource.driver-class-name=org.postgresql.Driver
spring.datasource.url=jdbc:postgresql://localhost:5432/postgres
spring.datasource.username=postgres
spring.datasource.password=rootroot
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true
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

## Redis、PostgreSQL、MongoDBサービスを含む`docker-compose.yaml`の追加

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

## `.gitignore`ファイルの追加

- `.gitignore`ファイルに`redis_data`、`postgres_data`、`mongo_data`ディレクトリを挿入します

## Mavenテストコマンドの実行

- プロジェクトが動作するかを確認するためにmaven clean testコマンドを実行します

```shell
./mvnw clean test
```

## Maven実行コマンド（オプション）

- （オプション）`docker-compose up -d`でサービスを開始し、`./mvnw spring-boot:run`でSpring Bootプロジェクトを実行し、`docker-compose rm -sf`でサービスを停止します。

## これをステップバイステップで進めましょう