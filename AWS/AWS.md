# AWS主要カテゴリ

| 分野 | 代表サービス | 一言でいうと |
|------|--------------|----------------|
| コンピューティング | Amazon EC2 | 仮想サーバー |
| コンピューティング | AWS Lambda | サーバーレス実行 |
| コンピューティング | AWS Fargate | Dockerコンテナの自動実行 |
| ストレージ | Amazon S3 | オブジェクト保管庫 |
| ストレージ | Amazon EBS | EC2用ディスク |
| ストレージ | Amazon EFS | 共有ファイルシステム |
| データベース | Amazon RDS | 管理付きDB |
| データベース | Amazon DynamoDB | 高速NoSQL |
| データベース | Amazon Aurora | 高性能RDB |
| ネットワーク | Amazon VPC | 仮想ネットワーク |
| ネットワーク | Elastic Load Balancing | 負荷分散 |
| ネットワーク | Amazon API Gateway | API公開 |
| CDN | Amazon CloudFront | 高速配信 |
| DNS | Amazon Route 53 | ドメイン管理 |
| セキュリティ | AWS IAM | 権限管理 |
| セキュリティ | AWS WAF | Web防御 |
| セキュリティ | AWS Shield | DDoS防御 |
| 監視 | Amazon CloudWatch | メトリクス監視 |
| 監視 | AWS CloudTrail | 操作ログ |
| 監視 | AWS Config | 設定監査 |
| サーバーレス | AWS Lambda | コード実行 |
| サーバーレス | Amazon EventBridge | イベント連携 |
| コンテナ | Amazon ECS | Docker運用 |
| コンテナ | Amazon EKS | Kubernetes運用 |
| IaC | AWS CloudFormation | 構成自動化 |
| IaC | AWS CDK | コードでIaC |
| メッセージング | Amazon SQS | キュー |
| メッセージング | Amazon SNS | 通知サービス |
| メッセージング | Amazon MQ | メッセージブローカー |
| 分析 | Amazon Athena | S3にSQL |
| 分析 | Amazon Redshift | DWH |
| 分析 | AWS Glue | ETL |
| 機械学習 | Amazon SageMaker | ML開発基盤 |
| ログ | Amazon OpenSearch | 検索・ログ分析 |
| ログ | AWS CloudWatch Logs | ログ収集 |
| 認証 | Amazon Cognito | 認証・ユーザー管理 |
| 開発者ツール | AWS CodeBuild | ビルド |
| 開発者ツール | AWS CodeDeploy | デプロイ |
| 開発者ツール | AWS CodePipeline | CI/CD |


### メトリクスとは
- クラウドやアプリの「健康状態」を知るための数値データ

例：
- CPU使用率（例：70%）
- メモリ使用量（例：3.2GB）
- ディスク容量（例：80%使用）
- リクエスト数（例：1秒あたり200件）
- エラー率（例：5%）

## AWSデメリット
### ベンダーロックイン
- **AWSのサービスに依存しすぎて、他社クラウドや自社環境へ移行しにくくなる状態**