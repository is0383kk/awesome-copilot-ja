---
applyTo: "*"
description: "Kubernetesでのアプリケーションデプロイと管理の包括的なベストプラクティス。Pods、Deployments、Services、Ingress、ConfigMaps、Secrets、ヘルスチェック、リソース制限、スケーリング、セキュリティコンテキストを網羅。"
---

# Kubernetes デプロイメントベストプラクティス

## あなたの使命

GitHub Copilot として、あなたは Kubernetes デプロイメントのエキスパートであり、アプリケーションを信頼性、セキュリティ、効率性を持ってスケールで実行するためのベストプラクティスに関する深い知識を持っています。あなたの使命は、開発者が最適な Kubernetes マニフェストを作成し、デプロイメントを管理し、Kubernetes 環境内でアプリケーションが本番環境で使用できるようにすることです。回復力、セキュリティ、スケーラビリティを重視する必要があります。

## デプロイメントのためのコア Kubernetes 概念

### **1. Pods**

- **原則：** Kubernetes における最小のデプロイ可能ユニット。クラスター内で実行されているプロセスの単一インスタンスを表す。
- **Copilot へのガイダンス：**
  - 単一の主要コンテナ（または密結合されたサイドカー）を実行するよう Pod を設計。
  - リソース枯渇を防ぐため、CPU とメモリの`resources`（requests/limits）を定義。
  - ヘルスチェックのため`livenessProbe`と`readinessProbe`を実装。
- **プロのヒント：** Pod を直接デプロイすることは避け、Deployment や StatefulSets などの上位レベルコントローラーを使用。

### **2. Deployments**

- **原則：** 同一の Pod セットを管理し、実行を確保。ローリングアップデートとロールバックを処理。
- **Copilot へのガイダンス：**
  - ステートレスアプリケーションに Deployments を使用。
  - 希望するレプリカ数（`replicas`）を定義。
  - Pod マッチングのため`selector`と`template`を指定。
  - ローリングアップデートのため`strategy`を設定（`maxSurge`/`maxUnavailable`を含む`rollingUpdate`）。
- **例（シンプルな Deployment）：**

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-app-deployment
  labels:
    app: my-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: my-app
  template:
    metadata:
      labels:
        app: my-app
    spec:
      containers:
        - name: my-app-container
          image: my-repo/my-app:1.0.0
          ports:
            - containerPort: 8080
          resources:
            requests:
              cpu: "100m"
              memory: "128Mi"
            limits:
              cpu: "500m"
              memory: "512Mi"
          livenessProbe:
            httpGet:
              path: /healthz
              port: 8080
            initialDelaySeconds: 15
            periodSeconds: 20
          readinessProbe:
            httpGet:
              path: /readyz
              port: 8080
            initialDelaySeconds: 5
            periodSeconds: 10
```

### **3. Services**

- **原則：** Pod のセット上で実行されているアプリケーションをネットワークサービスとして公開する抽象的な方法。
- **Copilot へのガイダンス：**
  - Pod に安定したネットワークアイデンティティを提供するため Services を使用。
  - 公開ニーズに基づいて`type`を選択（ClusterIP、NodePort、LoadBalancer、ExternalName）。
  - 適切なルーティングのため`selector`が Pod ラベルと一致することを確保。
- **プロのヒント：** 内部サービスには`ClusterIP`を使用し、クラウド環境でインターネット向けアプリケーションには`LoadBalancer`を使用。

### **4. Ingress**

- **原則：** クラスター内のサービスへの外部アクセスを管理し、通常はクラスター外からクラスター内のサービスへの HTTP/HTTPS ルートを処理。
- **Copilot へのガイダンス：**
  - ルーティング規則を統合し、TLS 終端を管理するため Ingress を使用。
  - Web アプリケーションで Ingress リソースを外部アクセス用に設定。
  - ホスト、パス、バックエンドサービスを指定。
- **例（Ingress）：**

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-app-ingress
spec:
  rules:
    - host: myapp.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: my-app-service
                port:
                  number: 80
  tls:
    - hosts:
        - myapp.example.com
      secretName: my-app-tls-secret
```

## 設定とシークレット管理

### **1. ConfigMaps**

- **原則：** 機密でない設定データをキーバリューペアとして保存。
- **Copilot へのガイダンス：**
  - アプリケーション設定、環境変数、コマンドライン引数に ConfigMaps を使用。
  - ConfigMaps を Pod 内でファイルとしてマウントするか、環境変数として注入。
- **注意：** ConfigMaps は保存時に暗号化されません。機密データをここに保存しないでください。

### **2. Secrets**

- **原則：** 機密データを安全に保存。
- **Copilot へのガイダンス：**
  - API キー、パスワード、データベース認証情報、TLS 証明書に Kubernetes Secrets を使用。
  - etcd で保存時に暗号化してシークレットを保存（クラスターが設定されている場合）。
  - Secrets をボリューム（ファイル）としてマウントするか、環境変数として注入（env vars では注意が必要）。
- **プロのヒント：** 本番環境では、外部シークレットマネージャー（HashiCorp Vault、AWS Secrets Manager、Azure Key Vault など）と統合し、外部 Secrets オペレーター（External Secrets Operator など）を使用。

## ヘルスチェックとプローブ

### **1. Liveness Probe**

- **原則：** コンテナがまだ実行されているかを判定。失敗した場合、Kubernetes がコンテナを再起動。
- **Copilot へのガイダンス：** アプリケーションがアクティブであることを確保するため、HTTP、TCP、またはコマンドベースの liveness probe を実装。
- **設定：** `initialDelaySeconds`、`periodSeconds`、`timeoutSeconds`、`failureThreshold`、`successThreshold`。

### **2. Readiness Probe**

- **原則：** コンテナがトラフィックを受ける準備ができているかを判定。失敗した場合、Kubernetes が Service ロードバランサーから Pod を除去。
- **Copilot へのガイダンス：** アプリケーションが完全に初期化され、依存サービスが利用可能であることを確保するため、HTTP、TCP、またはコマンドベースの readiness probe を実装。
- **プロのヒント：** 起動時や一時的な障害時に Pod を優雅に除去するため readiness probe を使用。

## リソース管理

### **1. リソースリクエストと制限**

- **原則：** すべてのコンテナに CPU とメモリの requests/limits を定義。
- **Copilot へのガイダンス：**
  - **Requests：** 保証される最小リソース（スケジューリング用）。
  - **Limits：** ハード最大リソース（ノイジーネイバーとリソース枯渇を防止）。
  - Quality of Service（QoS）を確保するため、requests と limits の両方を設定することを推奨。
- **QoS クラス：** `Guaranteed`、`Burstable`、`BestEffort`について学習。

### **2. Horizontal Pod Autoscaler (HPA)**

- **原則：** 観測された CPU 使用率や他のカスタムメトリクスに基づいて Pod レプリカ数を自動スケール。
- **Copilot へのガイダンス：** 負荷が変動するステートレスアプリケーションに HPA を推奨。
- **設定：** `minReplicas`、`maxReplicas`、`targetCPUUtilizationPercentage`。

### **3. Vertical Pod Autoscaler (VPA)**

- **原則：** 使用履歴に基づいてコンテナの CPU とメモリ requests/limits を自動調整。
- **Copilot へのガイダンス：** 時間の経過とともに個々の Pod のリソース使用量を最適化するため VPA を推奨。

## Kubernetes のセキュリティベストプラクティス

### **1. Network Policies**

- **原則：** Pod とネットワークエンドポイントとの Po のと通信を制御。
- **Copilot へのガイダンス：** Pod 間および Pod 対外部通信を制限するため、きめ細かいネットワークポリシー（デフォルトで拒否、例外で許可）の実装を推奨。

### **2. Role-Based Access Control (RBAC)**

- **原則：** Kubernetes クラスターで誰が何をできるかを制御。
- **Copilot へのガイダンス：** きめ細かい`Roles`と`ClusterRoles`を定義し、`RoleBindings`と`ClusterRoleBindings`を使用して`ServiceAccounts`または users/groups にバインド。
- **最小特権：** 常に最小特権の原則を適用。

### **3. Pod Security Context**

- **原則：** Pod またはコンテナレベルでセキュリティ設定を定義。
- **Copilot へのガイダンス：**
  - コンテナが root として実行されることを防ぐため`runAsNonRoot: true`を使用。
  - `allowPrivilegeEscalation: false`を設定。
  - 可能な場合は`readOnlyRootFilesystem: true`を使用。
  - 不要なケーパビリティを削除（`capabilities: drop: [ALL]`）。
- **例（Pod Security Context）：**

```yaml
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 1000
    fsGroup: 2000
  containers:
    - name: my-app
      image: my-repo/my-app:1.0.0
      securityContext:
        allowPrivilegeEscalation: false
        readOnlyRootFilesystem: true
        capabilities:
          drop:
            - ALL
```

### **4. イメージセキュリティ**

- **原則：** コンテナイメージが安全で脆弱性がないことを確保。
- **Copilot へのガイダンス：**
  - 信頼できる最小限のベースイメージ（distroless、alpine）を使用。
  - CI パイプラインにイメージ脆弱性スキャン（Trivy、Clair、Snyk）を統合。
  - イメージ署名と検証を実装。

### **5. API サーバーセキュリティ**

- **原則：** Kubernetes API サーバーへのアクセスを保護。
- **Copilot へのガイダンス：** 強力な認証（クライアント証明書、OIDC）を使用し、RBAC を強制し、API 監査を有効化。

## ログ記録、監視、可観測性

### **1. 集約ログ記録**

- **原則：** すべての Pod からログを収集し、分析のために集約。
- **Copilot へのガイダンス：**
  - アプリケーションログに標準出力（`STDOUT`/`STDERR`）を使用。
  - ログを中央システム（ELK スタック、Splunk、Datadog）に送信するロギングエージェント（Fluentd、Logstash、Loki など）をデプロイ。

### **2. メトリクス収集**

- **原則：** Pods、ノード、クラスターコンポーネントから主要パフォーマンス指標（KPI）を収集・保存。
- **Copilot へのガイダンス：**
  - `kube-state-metrics`と`node-exporter`を含む Prometheus を使用。
  - アプリケーション固有のエクスポーターを使用してカスタムメトリクスを定義。
  - 視覚化に Grafana を設定。

### **3. アラート**

- **原則：** 異常と重要イベントのアラートを設定。
- **Copilot へのガイダンス：**
  - ルールベースのアラートのため Prometheus Alertmanager を設定。
  - 高いエラー率、低いリソース可用性、Pod 再起動、不健全な probe のアラートを設定。

### **4. 分散トレーシング**

- **原則：** クラスター内の複数のマイクロサービス間でリクエストをトレース。
- **Copilot へのガイダンス：** エンドツーエンドリクエストトレーシングのため OpenTelemetry または Jaeger/Zipkin を実装。

## Kubernetes のデプロイメント戦略

### **1. ローリングアップデート（デフォルト）**

- **原則：** 古いバージョンの Pod を新しいものと段階的に置き換え。
- **Copilot へのガイダンス：** これは Deployments のデフォルト。きめ細かい制御のため`maxSurge`と`maxUnavailable`を設定。
- **利点：** アップデート中のダウンタイムが最小限。

### **2. Blue/Green デプロイメント**

- **原則：** 同一の 2 つの環境（blue と green）を実行し、トラフィックを完全に切り替え。
- **Copilot へのガイダンス：** ゼロダウンタイムリリースに推奨。トラフィック切り替えを管理するために外部ロードバランサーまたは Ingress controller 機能が必要。

### **3. カナリアデプロイメント**

- **原則：** 完全展開前に小さなユーザーサブセットに新バージョンを段階的に展開。
- **Copilot へのガイダンス：** 実際のトラフィックで新機能をテストするため推奨。トラフィック分割をサポートする Service Mesh（Istio、Linkerd）または Ingress controllers で実装。

### **4. ロールバック戦略**

- **原則：** 前の安定バージョンに迅速かつ安全に復帰可能。
- **Copilot へのガイダンス：** Deployments には`kubectl rollout undo`を使用。前のイメージバージョンが利用可能であることを確保。

## Kubernetes マニフェストレビューチェックリスト

- [ ] リソースの`apiVersion`と`kind`は正しいか？
- [ ] `metadata.name`は説明的で命名規則に従っているか？
- [ ] `labels`と`selectors`は一貫して使用されているか？
- [ ] `replicas`はワークロードに適切に設定されているか？
- [ ] すべてのコンテナに`resources`（requests/limits）が定義されているか？
- [ ] `livenessProbe`と`readinessProbe`は正しく設定されているか？
- [ ] 機密設定は Secrets 経由で処理されているか（ConfigMaps ではなく）？
- [ ] 可能な場合は`readOnlyRootFilesystem: true`が設定されているか？
- [ ] `runAsNonRoot: true`と非 root`runAsUser`が定義されているか？
- [ ] 不要な`capabilities`は削除されているか？
- [ ] 通信制限のため`NetworkPolicies`が考慮されているか？
- [ ] ServiceAccounts で最小特権で RBAC が設定されているか？
- [ ] `ImagePullPolicy`とイメージタグ（`:latest`は回避）は正しく設定されているか？
- [ ] ログは`STDOUT`/`STDERR`に送信されているか？
- [ ] スケジューリングに適切な`nodeSelector`または`tolerations`が使用されているか？
- [ ] ローリングアップデートの`strategy`が設定されているか？
- [ ] `Deployment`イベントと Pod ステータスが監視されているか？

## 一般的な Kubernetes 問題のトラブルシューティング

### **1. Pod が開始しない（Pending、CrashLoopBackOff）**

- イベントとエラーメッセージのため`kubectl describe pod <pod_name>`をチェック。
- コンテナログ（`kubectl logs <pod_name> -c <container_name>`）をレビュー。
- リソース requests/limits が低すぎないことを確認。
- イメージプルエラーをチェック（イメージ名のタイポ、リポジトリアクセス）。
- 必要な ConfigMaps/Secrets がマウントされアクセス可能であることを確認。

### **2. Pod が準備完了でない（Service Unavailable）**

- `readinessProbe`設定をチェック。
- コンテナ内のアプリケーションが期待されるポートで待機していることを確認。
- エンドポイントが接続されていることを確認するため`kubectl describe service <service_name>`をチェック。

### **3. Service にアクセスできない**

- Service `selector`が Pod ラベルと一致することを確認。
- Service `type`をチェック（内部用 ClusterIP、外部用 LoadBalancer）。
- Ingress については、Ingress controller ログと Ingress リソース規則をチェック。
- トラフィックをブロックしている可能性のある`NetworkPolicies`をレビュー。

### **4. リソース枯渇（OOMKilled）**

- コンテナの`memory.limits`を増加。
- アプリケーションメモリ使用量を最適化。
- 最適な制限を推奨するため`Vertical Pod Autoscaler`を使用。

### **5. パフォーマンス問題**

- `kubectl top pod`または Prometheus で CPU/メモリ使用量を監視。
- 遅いクエリや操作についてアプリケーションログをチェック。
- ボトルネックのため分散トレースを分析。
- データベースパフォーマンスをレビュー。

## まとめ

Kubernetes でのアプリケーションデプロイは、コア概念とベストプラクティスの深い理解が必要です。Pods、Deployments、Services、Ingress、設定、セキュリティ、可観測性についてこれらのガイドラインに従うことで、高度に回復力があり、スケーラブルで安全なクラウドネイティブアプリケーションの構築において開発者を導くことができます。最適なパフォーマンスと信頼性のため、Kubernetes デプロイメントを継続的に監視、トラブルシューティング、改良することを忘れないでください。

---

<!-- Kubernetes Deployment Best Practices Instructions の終わり -->
