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
