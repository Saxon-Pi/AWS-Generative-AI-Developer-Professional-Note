## 目次
- [PrivateLink](#privatelink)
- [API Gateway マッピングテンプレート](#api-gateway-マッピングテンプレート)

---

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

---

# API Gateway マッピングテンプレート

## 概要
- API Gatewayでリクエスト/レスポンスのデータ変換を行う仕組み
- クライアントとバックエンドのデータ形式を仲介する
- REST APIではVTL（Velocity Template Language）で記述

---

## 基本の役割

- リクエストの整形
- レスポンスの整形
- フィールド抽出・変換
- 軽い条件分岐

---

## 処理フロー

クライアントリクエスト  
↓  
Mapping Template（リクエスト変換）  
↓  
Integration（Lambda / HTTP）  
↓  
Mapping Template（レスポンス変換）  
↓  
クライアントレスポンス  

---

## できること

- JSONの特定フィールド抽出
- query / path パラメータの変換
- JSON構造の変更
- 条件に応じた値の設定
- 簡単なルーティング情報付与

---

## 例

入力：
```json
{
  "message": "Hello"
}
```

変換：
```json
{
  "text": "Hello",
  "language": "en"
}
```

---

## 利用シーン

- API仕様の差分吸収
- バックエンドの変更を隠蔽
- 入力内容に応じた処理振り分け
- 軽量な前処理

---

## 今回の問題のポイント

- 入力JSONから言語情報を取得
- マッピングテンプレートで整形
- Lambdaにルーティング情報を渡す
- Lambdaで適切なモデル（Claude / Titan）を呼び分け

---

## 特徴

- 軽量で高速
- インフラレベルで処理できる
- コード不要（VTLで記述）

---

## 制約

- 複雑なロジックには不向き
- 可読性が低くなりがち
- デバッグが難しい

---

## 試験ポイント

- 「API Gatewayでデータ変換」→ マッピングテンプレート
- 「軽い前処理・整形」→ マッピングテンプレート
- 「複雑な処理」→ Lambda
- 「既存システム変更を避ける」→ API Gatewayで吸収

---

## よくある誤り

- 複雑なビジネスロジックを実装 → NG
- Lambdaなしで全処理を完結 → NG
- 単なるルーティングサービスと誤解 → NG

---

## 一言まとめ

- マッピングテンプレート = API Gatewayで行う軽量なデータ変換レイヤ
