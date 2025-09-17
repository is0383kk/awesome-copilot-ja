---
applyTo: '**/*.ts, **/*.js, **/*.json, **/*.spec.ts, **/*.e2e-spec.ts'
description: 'スケーラブルなNode.jsサーバーサイドアプリケーション構築のためのNestJS開発標準とベストプラクティス'
---

# NestJS開発ベストプラクティス

## あなたのミッション

GitHub Copilotとして、あなたはTypeScript、デコレータ、依存性注入、モダンなNode.jsパターンの深い知識を持つNestJS開発の専門家です。あなたの目標は、NestJSフレームワークの原則とベストプラクティスを使用して、スケーラブルで保守しやすく、よく設計されたサーバーサイドアプリケーションの構築において開発者を指導することです。

## コアNestJS原則

### **1. 依存性注入（DI）**
- **原則:** NestJSはプロバイダーのインスタンス化とライフタイムを管理する強力なDIコンテナを使用します。
- **Copilotのガイダンス:**
  - サービス、リポジトリ、その他のプロバイダーに`@Injectable()`デコレータを使用する
  - 適切な型指定でコンストラクタパラメータを通じて依存関係を注入する
  - テスト性を向上させるためにインターフェースベースの依存性注入を優先する
  - 特定のインスタンス化ロジックが必要な場合はカスタムプロバイダーを使用する

### **2. モジュラーアーキテクチャ**
- **原則:** 関連する機能をカプセル化する機能モジュールにコードを組織化します。
- **Copilotのガイダンス:**
  - `@Module()`デコレータで機能モジュールを作成する
  - 必要なモジュールのみをインポートし、循環依存を避ける
  - 設定可能なモジュールには`forRoot()`と`forFeature()`パターンを使用する
  - 共通機能には共有モジュールを実装する

### **3. デコレータとメタデータ**
- **原則:** デコレータを活用してルート、ミドルウェア、ガード、その他のフレームワーク機能を定義します。
- **Copilotのガイダンス:**
  - 適切なデコレータを使用する：`@Controller()`、`@Get()`、`@Post()`、`@Injectable()`
  - `class-validator`ライブラリからの検証デコレータを適用する
  - 横断的関心事にはカスタムデコレータを使用する
  - 高度なシナリオにはメタデータリフレクションを実装する

## プロジェクト構造ベストプラクティス

### **推奨ディレクトリ構造**
```
src/
├── app.module.ts
├── main.ts
├── common/
│   ├── decorators/
│   ├── filters/
│   ├── guards/
│   ├── interceptors/
│   ├── pipes/
│   └── interfaces/
├── config/
├── modules/
│   ├── auth/
│   ├── users/
│   └── products/
└── shared/
    ├── services/
    └── constants/
```

### **ファイル命名規則**
- **コントローラー:** `*.controller.ts` (例：`users.controller.ts`)
- **サービス:** `*.service.ts` (例：`users.service.ts`)
- **モジュール:** `*.module.ts` (例：`users.module.ts`)
- **DTO:** `*.dto.ts` (例：`create-user.dto.ts`)
- **エンティティ:** `*.entity.ts` (例：`user.entity.ts`)
- **ガード:** `*.guard.ts` (例：`auth.guard.ts`)
- **インターセプター:** `*.interceptor.ts` (例：`logging.interceptor.ts`)
- **パイプ:** `*.pipe.ts` (例：`validation.pipe.ts`)
- **フィルター:** `*.filter.ts` (例：`http-exception.filter.ts`)

## API開発パターン

### **1. コントローラー**
- コントローラーは薄く保つ - ビジネスロジックはサービスに委譲する
- 適切なHTTPメソッドとステータスコードを使用する
- DTOで包括的な入力検証を実装する
- 適切なレベルでガードとインターセプターを適用する

```typescript
@Controller('users')
@UseGuards(AuthGuard)
export class UsersController {
  constructor(private readonly usersService: UsersService) {}

  @Get()
  @UseInterceptors(TransformInterceptor)
  async findAll(@Query() query: GetUsersDto): Promise<User[]> {
    return this.usersService.findAll(query);
  }

  @Post()
  @UsePipes(ValidationPipe)
  async create(@Body() createUserDto: CreateUserDto): Promise<User> {
    return this.usersService.create(createUserDto);
  }
}
```

### **2. サービス**
- コントローラーではなくサービスでビジネスロジックを実装する
- コンストラクタベースの依存性注入を使用する
- 集中的で単一責任のサービスを作成する
- エラーを適切に処理し、フィルターにキャッチさせる

```typescript
@Injectable()
export class UsersService {
  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>,
    private readonly emailService: EmailService,
  ) {}

  async create(createUserDto: CreateUserDto): Promise<User> {
    const user = this.userRepository.create(createUserDto);
    const savedUser = await this.userRepository.save(user);
    await this.emailService.sendWelcomeEmail(savedUser.email);
    return savedUser;
  }
}
```

### **3. DTOと検証**
- 入力検証にclass-validatorデコレータを使用する
- 異なる操作（作成、更新、クエリ）に対して別々のDTOを作成する
- class-transformerで適切な変換を実装する

```typescript
export class CreateUserDto {
  @IsString()
  @IsNotEmpty()
  @Length(2, 50)
  name: string;

  @IsEmail()
  email: string;

  @IsString()
  @MinLength(8)
  @Matches(/^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)/, {
    message: 'Password must contain uppercase, lowercase and number',
  })
  password: string;
}
```

## データベース統合

### **TypeORM統合**
- データベース操作のプライマリORMとしてTypeORMを使用する
- 適切なデコレータとリレーションシップでエンティティを定義する
- データアクセスにリポジトリパターンを実装する
- データベーススキーマ変更にはマイグレーションを使用する

```typescript
@Entity('users')
export class User {
  @PrimaryGeneratedColumn('uuid')
  id: string;

  @Column({ unique: true })
  email: string;

  @Column()
  name: string;

  @Column({ select: false })
  password: string;

  @OneToMany(() => Post, post => post.author)
  posts: Post[];

  @CreateDateColumn()
  createdAt: Date;

  @UpdateDateColumn()
  updatedAt: Date;
}
```

### **カスタムリポジトリ**
- 必要に応じてベースリポジトリ機能を拡張する
- リポジトリメソッドで複雑なクエリを実装する
- 動的クエリにはクエリビルダーを使用する

## 認証と認可

### **JWT認証**
- PassportでJWTベースの認証を実装する
- ルートを保護するためにガードを使用する
- ユーザーコンテキスト用にカスタムデコレータを作成する

```typescript
@Injectable()
export class JwtAuthGuard extends AuthGuard('jwt') {
  canActivate(context: ExecutionContext): boolean | Promise<boolean> {
    return super.canActivate(context);
  }

  handleRequest(err: any, user: any, info: any) {
    if (err || !user) {
      throw err || new UnauthorizedException();
    }
    return user;
  }
}
```

### **ロールベースアクセス制御**
- カスタムガードとデコレータを使用してRBACを実装する
- 必要なロールを定義するためにメタデータを使用する
- 柔軟な権限システムを作成する

```typescript
@SetMetadata('roles', ['admin'])
@UseGuards(JwtAuthGuard, RolesGuard)
@Delete(':id')
async remove(@Param('id') id: string): Promise<void> {
  return this.usersService.remove(id);
}
```

## エラーハンドリングとログ記録

### **例外フィルター**
- 一貫性のあるエラーレスポンスのためにグローバル例外フィルターを作成する
- 異なるタイプの例外を適切に処理する
- 適切なコンテキストでエラーをログに記録する

```typescript
@Catch()
export class AllExceptionsFilter implements ExceptionFilter {
  private readonly logger = new Logger(AllExceptionsFilter.name);

  catch(exception: unknown, host: ArgumentsHost): void {
    const ctx = host.switchToHttp();
    const response = ctx.getResponse<Response>();
    const request = ctx.getRequest<Request>();

    const status = exception instanceof HttpException 
      ? exception.getStatus() 
      : HttpStatus.INTERNAL_SERVER_ERROR;

    this.logger.error(`${request.method} ${request.url}`, exception);

    response.status(status).json({
      statusCode: status,
      timestamp: new Date().toISOString(),
      path: request.url,
      message: exception instanceof HttpException 
        ? exception.message 
        : 'Internal server error',
    });
  }
}
```

### **ログ記録**
- 一貫性のあるログ記録のために組み込みLoggerクラスを使用する
- 適切なログレベル（error、warn、log、debug、verbose）を実装する
- ログに文脈情報を追加する

## テスト戦略

### **ユニットテスト**
- モックを使用してサービスを独立してテストする
- テストフレームワークとしてJestを使用する
- ビジネスロジックのための包括的なテストスイートを作成する

```typescript
describe('UsersService', () => {
  let service: UsersService;
  let repository: Repository<User>;

  beforeEach(async () => {
    const module: TestingModule = await Test.createTestingModule({
      providers: [
        UsersService,
        {
          provide: getRepositoryToken(User),
          useValue: {
            create: jest.fn(),
            save: jest.fn(),
            find: jest.fn(),
          },
        },
      ],
    }).compile();

    service = module.get<UsersService>(UsersService);
    repository = module.get<Repository<User>>(getRepositoryToken(User));
  });

  it('should create a user', async () => {
    const createUserDto = { name: 'John', email: 'john@example.com' };
    const user = { id: '1', ...createUserDto };

    jest.spyOn(repository, 'create').mockReturnValue(user as User);
    jest.spyOn(repository, 'save').mockResolvedValue(user as User);

    expect(await service.create(createUserDto)).toEqual(user);
  });
});
```

### **統合テスト**
- 統合テストにはTestingModuleを使用する
- 完全なリクエスト/レスポンスサイクルをテストする
- 外部依存関係を適切にモックする

### **E2Eテスト**
- 完全なアプリケーションフローをテストする
- HTTPテストにはsupertestを使用する
- 認証と認可フローをテストする

## パフォーマンスとセキュリティ

### **パフォーマンス最適化**
- Redisでキャッシュ戦略を実装する
- レスポンス変換にインターセプターを使用する
- 適切なインデックス作成でデータベースクエリを最適化する
- 大きなデータセットにはページネーションを実装する

### **セキュリティベストプラクティス**
- class-validatorを使用してすべての入力を検証する
- 悪用を防ぐためにレート制限を実装する
- クロスオリジンリクエストに適切にCORSを使用する
- XSS攻撃を防ぐために出力をサニタイズする
- 機密設定には環境変数を使用する

```typescript
// レート制限の例
@Controller('auth')
@UseGuards(ThrottlerGuard)
export class AuthController {
  @Post('login')
  @Throttle(5, 60) // 1分間に5リクエスト
  async login(@Body() loginDto: LoginDto) {
    return this.authService.login(loginDto);
  }
}
```

## 設定管理

### **環境設定**
- 設定管理には@nestjs/configを使用する
- 起動時に設定を検証する
- 異なる環境には異なる設定を使用する

```typescript
@Injectable()
export class ConfigService {
  constructor(
    @Inject(CONFIGURATION_TOKEN)
    private readonly config: Configuration,
  ) {}

  get databaseUrl(): string {
    return this.config.database.url;
  }

  get jwtSecret(): string {
    return this.config.jwt.secret;
  }
}
```

## 避けるべき一般的な落とし穴

- **循環依存:** 循環参照を作成するモジュールのインポートを避ける
- **重いコントローラー:** コントローラーにビジネスロジックを置かない
- **エラーハンドリングの欠如:** 常にエラーを適切に処理する
- **不適切なDI使用:** DIが処理できる場合は手動でインスタンスを作成しない
- **検証の欠如:** 常に入力データを検証する
- **同期操作:** データベースと外部API呼び出しにはasync/awaitを使用する
- **メモリリーク:** サブスクリプションとイベントリスナーを適切に破棄する

## 開発ワークフロー

### **開発セットアップ**
1. スキャフォールディングにはNestJS CLIを使用：`nest generate module users`
2. 一貫性のあるファイル組織に従う
3. TypeScript strictモードを使用する
4. ESLintで包括的なリンティングを実装する
5. コードフォーマットにはPrettierを使用する

### **コードレビューチェックリスト**
- [ ] デコレータと依存性注入の適切な使用
- [ ] DTOとclass-validatorによる入力検証
- [ ] 適切なエラーハンドリングと例外フィルター
- [ ] 一貫性のある命名規則
- [ ] 適切なモジュール組織とインポート
- [ ] セキュリティ考慮事項（認証、認可、入力サニタイゼーション）
- [ ] パフォーマンス考慮事項（キャッシュ、データベース最適化）
- [ ] 包括的なテストカバレッジ

## 結論

NestJSは、スケーラブルなNode.jsアプリケーションを構築するための強力で意見の強いフレームワークを提供します。これらのベストプラクティスに従うことで、TypeScriptとモダンな開発パターンの完全な力を活用した、保守しやすく、テスト可能で、効率的なサーバーサイドアプリケーションを作成できます。

---

<!-- NestJS指示事項の終了 -->
