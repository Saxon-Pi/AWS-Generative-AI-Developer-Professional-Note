# Amazon Q Developer / Amazon Q Business

## 概要

### Amazon Q Developer
- 開発者向けのAIコーディングアシスタント
- IDE上でコード生成・改善・テスト生成を支援
- AWSサービスとの連携に強い

### Amazon Q Business
- 社内データを横断検索するAIアシスタント
- RAGベースのナレッジ検索・業務支援
- ドキュメント・SaaS・社内システムと連携

---

## Amazon Q Developer

### 主な機能
- コード補完 / 自動生成
- リファクタリング提案
- テストコード生成
- バグ検出 / セキュリティチェック
- AWS SDK / API利用方法の支援

### ユースケース
- 新機能の実装支援
- 既存コードの改善
- テスト自動生成
- AWSサービス利用コードの作成

### 特徴
- IDE統合（VS Codeなど）
- プロジェクトコンテキストを理解
- AWSベストプラクティスに基づく提案

### 制約
- 実際のAWSリソース操作は行わない
- 状況分析・提案まで（実行は開発者）

---

## Amazon Q Business

### 主な機能
- 社内ドキュメント検索
- ナレッジベース統合
- 質問応答（チャット形式）
- 業務支援（FAQ・手順案内）

### ユースケース
- 社内ナレッジ検索
- ドキュメント横断検索
- 社内問い合わせ対応
- 業務効率化

### 特徴
- RAGベース
- SharePoint / Confluence / S3 などと連携
- アクセス制御（ACL）対応

---

## 比較

|項目|Q Developer|Q Business|
|--|--|--|
用途|開発支援|業務・検索支援|
対象|コード|ドキュメント|
利用場所|IDE|チャットUI|
技術|コード生成|RAG|
役割|作る|探す|

---

## 他サービスとの関係

- Q Developer
  - 開発支援ツール
  - 旧CodeWhispererの後継

- Q Business
  - ナレッジ検索
  - Bedrock Knowledge Baseに近い役割

- Agent
  - 実際に処理を実行する仕組み（別カテゴリ）

---

## 試験ポイント

- コード生成・改善 → Q Developer
- テスト生成 → Q Developer
- 社内検索 → Q Business
- ドキュメント統合 → Q Business

---

## よくある誤り

- Q Developerで社内検索 → NG
- Q Businessでコード生成 → NG
- Q Developerがリソース操作する → NG

---

## 一言まとめ

- Q Developer = コードを書くAI
- Q Business = 社内情報を探すAI

---

# Amazon Augmented AI（A2I）

## 概要
- ML推論結果に人間レビューを組み込むサービス
- Human-in-the-Loop（HITL）を実現するための基盤
- 品質・説明責任・コンプライアンス対応に重要

---

## できること

- 人間レビューのワークフロー化
- 低信頼予測のみレビューに回す
- 全件レビュー（監査用途）
- 承認 / 差し戻しの管理
- ワークフォース（作業者）の管理

---

## Human Review Loop

モデル推論  
↓  
条件判定（信頼度など）  
↓  
A2Iで人間レビュー  
↓  
結果確定（承認 / 修正）  

---

## ワークフォース

- Private（社内）
- Vendor（外部委託）
- Amazon Mechanical Turk

---

## 利用シーン

- 保険審査・クレーム対応
- 医療文書レビュー
- メール生成の確認
- コンプライアンスチェック
- OCR結果の確認

---

## Bedrockとの関係

- Bedrockとネイティブ統合された中心機能ではない
- Bedrockの出力をA2Iに渡してレビュー可能
- Step Functions / Lambdaで連携する構成が一般的

---

## Bedrockとの違い

|用途|サービス|
|--|--|
生成|Bedrock|
人間レビュー|A2I|
ワークフロー制御|Step Functions|

---

## 試験ポイント

- 「人間による確認」→ A2I
- 「承認・差し戻し」→ A2I
- 「HITL」→ A2I
- 「生成 → 承認フロー」→ Step Functions + A2I

---

## よくある誤り

- Bedrockが人間レビューを直接管理する → NG
- A2Iがモデル推論を行う → NG
- Step FunctionsだけでレビューUIまで提供される → NG

---

## 一言まとめ

- A2I = 人間レビューをシステムとして組み込むサービス
