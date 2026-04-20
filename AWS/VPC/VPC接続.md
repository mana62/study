# VPC接続方法
# ① VPÇ外部と接続する時：VPCエンドポイント
インターネットを通らずに、**VPCからAWSサービスへプライベート接続する仕組み**

つまり、
- NAT Gateway不要
- IGW不要
- AWS内部ネットワークで通信
できる

## VPCエンドポイントの種類
### ① ゲートウェイ型エンドポイント
対象：
- Amazon S3
- Amazon DynamoDB

特徴：
- **ルートテーブルに追加**して使う
- 無料（一般に）
- シンプル

使い方イメージ：
`Private subnet → Gateway Endpoint → S3`

### ② プライベートリンク型（インターフェイス型）エンドポイント
正式には **Interface Endpoint**

特徴：
- サブネット内にENI作成
- Private IP持つ
- 多数AWSサービス対応
- 他社SaaS接続にも使える

対象例：
- AWS Systems Manager
- Amazon CloudWatch
- 独自サービス
- S3

イメージ：
`EC2 → Private IP Endpoint → AWSサービス`


## 比較
| 項目    | Gateway型      | Interface型 |
| ----- | ------------- | ---------- |
| 主対象   | S3 / DynamoDB | 多数サービス     |
| 接続方法  | ルートテーブル       | ENI        |
| コスト   | 安価/無料         | 時間課金あり     |
| DNS利用 | 少なめ           | 多い         |


# ②2つのVPC同士をプライベートIPで直接つなぐ機能：VPC Peering

別々のVPCが2つあると通常は通信できない
```bash
VPC-A（10.0.0.0/16）
VPC-B（172.16.0.0/16）
```
👉 この2つを仲良くつなぐのが **VPC Peering**

## メリット
| 項目           | 内容       |
| ------------ | -------- |
| インターネット不要    | 安全       |
| 低遅延          | AWS内部通信  |
| Private IP通信 | 内部設計しやすい |
| 別アカウント接続可    | 組織利用向き   |

## 設定で必要なもの
### ① Peering作成
```bash
VPC-A ↔ VPC-B
```
### ② ルートテーブル追加
例：
```bash
VPC-A route:
172.16.0.0/16 → Peering

VPC-B route:
10.0.0.0/16 → Peering
```
### ③ Security Group / NACL許可

## ⚠️ 注意点
- CIDR重複NG
```bash
VPC-A 10.0.0.0/16
VPC-B 10.0.0.0/16
```
同じだと基本NG

- 中継不可（Transitive Routingなし）
```bash
A ↔ B ↔ C
```
でもA から C へ B経由は基本できない

## VPC peering まとめ
⚫︎ **2つのVPCだけ**接続したい
➡ **VPC Peering**

⚫︎**多数**VPCを集約したい
➡ **Transit Gateway（トランジット ゲートウェイ）**

# ③ AWSと会社（オンプレミス）をつなぐ方法:2種類
- 会社の社内サーバーとAWSをつなぎたい時に使う

## ① VPC Direct Conect
- **AWSへ専用回線で直結**するサービス

```bash
会社 ───── 専用線 ───── AWS
```
### 特徴
| 項目     | 内容    |
| ------ | ----- |
| 通信品質   | 高い    |
| 安定性    | 高い    |
| 速度     | 高速    |
| セキュリティ | 高い    |
| コスト    | 高め    |
| 導入速度   | 時間かかる |


## ② AWS VPN
- **インターネット回線を使って暗号化**接続

```bash
会社 → Internet(VPN) → AWS
```

### 特徴
| 項目     | 内容    |
| ------ | ----- |
| 通信品質   | 回線依存  |
| 安定性    | 普通    |
| 速度     | 普通    |
| セキュリティ | 暗号化あり |
| コスト    | 安い    |
| 導入速度   | 速い    |

## まとめ
| 接続方式                 | 一言           |
| -------------------- | ------------ |
| AWS Direct Connect   | 専用線で直結       |
| AWS Site-to-Site VPN | インターネット経由VPN |