# PrivateLink

## 概要
- AWSサービスへインターネットを経由せずに接続する仕組み
- VPC内からAWSサービスにプライベート接続できる

## 正体
- PrivateLink = Interface VPC Endpoint

## 通信イメージ
### 通常
VPC → Internet → AWSサービス

### PrivateLink
VPC → AWS内部ネットワーク → AWSサービス

## 仕組み
- VPC内にENI（Elastic Network Interface）を作成
- プライベートIPでアクセス
- DNSが内部IPに解決される

## 必須条件
- Interface型のVPC Endpointを作成
- 作成するとPrivateLink通信になる

## 主な用途
- インターネット禁止環境（医療・金融など）
- 機密データの保護
- 閉域ネットワーク構成

## Bedrockとの関係
- BedrockはPrivateLink対応
- VPC Endpoint経由で推論APIを呼び出し可能

## Gateway Endpointとの違い
- Gateway Endpoint
  - S3 / DynamoDB専用
  - ルートテーブルで制御
  - PrivateLinkではない

- PrivateLink（Interface Endpoint）
  - 汎用AWSサービス
  - ENIベース通信

## 他サービスとの使い分け
- AWSサービス接続 → PrivateLink
- S3 / DynamoDB → Gateway Endpoint
- VPC同士 → VPC Peering / Transit Gateway
- インターネット経由 → NAT Gateway

## 試験ポイント
- 「インターネット禁止」「閉域」「機密データ」 → PrivateLink
- PrivateLink = Interface Endpoint

## よくある誤り
- PrivateLinkでVPC同士接続（誤り）
- Gateway Endpoint = PrivateLink（誤り）

## 一言まとめ
- PrivateLink = インターネットを使わないAWSサービス接続
