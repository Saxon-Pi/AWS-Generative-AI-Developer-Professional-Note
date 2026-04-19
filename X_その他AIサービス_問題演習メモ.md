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

---

# Amazon Lex と Synonyms（スロット改善）

## 概要
- Amazon Lex はルールベース寄りの自然言語理解サービス
- 意味理解（Embedding）ではなく「定義済み値 + synonyms」で解釈する
- 類義語を認識させるには synonyms の設定が重要

---

## 問題の本質

### ユーザー入力
- thrill-seeking
- sightseeing
- chill

### 既存カテゴリ
- adventure
- culture
- relaxation

→ 意味は近いが文字列が一致しないため認識できない

---

## なぜ起きるか

- Lexは意味ベースではない
- 文字列一致・辞書ベースのマッチング
- 類義語を自動で理解しない

---

## 解決策：Synonyms

### custom slot type

例：

Slot: vacation_type

値（canonical values）:
- relaxation
- adventure
- culture

synonyms:
- relaxation → chill
- adventure → thrill-seeking
- culture → sightseeing

---

## 効果

- chill → relaxation に変換される
- thrill-seeking → adventure に変換される
- 意味の近い入力を正しく処理できる

---

## なぜこれが最適か

問題の制約：
- Lambda変更不可
- DB変更不可
- 即時対応必要

→ Lex設定のみで解決できるのが唯一の手段

---

## NG対応

- TitanなどEmbedding導入 → 即時不可
- Lambdaで変換 → 制約違反
- DB側修正 → 制約違反

---

## 試験ポイント

- 「似た意味が認識されない」→ synonyms
- 「slot type」→ 値と類義語の管理
- 「即時対応」→ 設定変更で解決

---

## Generative AIとの違い

|項目|Lex|Bedrock|
|--|--|--|
理解方法|辞書ベース|意味ベース（Embedding）|
柔軟性|低|高|
類義語対応|手動|自動（ある程度）|

---

## 一言まとめ

- Lexは意味を理解しないため、類義語はsynonymsで定義する必要がある

---

# Comprehendにおける Offsets / Labels の整理

## 概要
- Offsets / Labels は「独立した機能」ではない
- 各分析機能の「出力項目（属性）」として返される情報

---

## 全体構造

### ① 分析機能（何をするか）
- Entity Detection
- PII Detection
- Sentiment Analysis
- Key Phrase Detection
- Syntax Analysis
- Custom Classification

---

### ② 出力（何が返るか）

- Labels（Type）
  - エンティティの種類
  - 例：PERSON, LOCATION, EMAIL など

- Offsets（BeginOffset / EndOffset）
  - テキスト内の位置情報
  - 例：開始位置・終了位置

---

## イメージ

Entity Detection の出力例：

```json
{
  "Text": "John",
  "Type": "PERSON",      ← Labels
  "BeginOffset": 0,      ← Offsets
  "EndOffset": 4
}
```

---

## 今回の問題のポイント

### 要件
- PIIの位置が必要 → Offsets
- PIIの種類が必要 → Labels

---

## 試験での考え方

- 「どの分析機能か」ではなく
- 「どんな出力が必要か」で判断する

---

## よくあるトリック

- 「Offsets analysis」
- 「Labels analysis」

→ 実際には存在しない“機能名風の表現”

---

## 一言まとめ

- Offsets = 位置
- Labels = 種類
- どちらも「分析結果の属性」

---

# LLMアーキテクチャ比較 （Bedrock / LangChain / LangGraph / Step Functions）

## 概要
LLMアプリ開発でよく使う4つの選択肢：
- Bedrock Agents（AWSマネージド）
- LangChain（OSSフレームワーク）
- LangGraph（エージェント拡張）
- Step Functions（AWSワークフロー）

👉 それぞれ役割が違うのが重要

---

## 全体の位置づけ

| レイヤ | ツール | 役割 |
|-------|------|------|
| インフラ | AWS | データ・実行基盤 |
| LLM | Bedrock | モデル実行 |
| アプリロジック | LangChain | RAG / ツール連携 |
| エージェント制御 | LangGraph | 状態付き推論 |
| 業務ワークフロー | Step Functions | システム連携 |

---

## Bedrock Agents
- AWS完全マネージド
- 簡単にエージェント構築
- Action GroupでAPI呼び出し

向いている：
- FAQボット
- シンプルRAG

弱点：
- 柔軟性が低い

---

## LangChain
- OSSのLLMフレームワーク
- RAG / ツール連携

できること：
- embedding / retrieval
- LLM切替
- ツール実行

👉 LLMアプリのロジック層

---

## LangGraph
- LangChain拡張
- 状態付きエージェント

できること：
- state管理
- 分岐・ループ
- checkpoint
- rollback / replay

👉 エージェントの本格制御

---

## Step Functions
- AWSワークフロー
- ステートマシン

できること：
- Lambda連携
- retry / error handling
- 並列処理

👉 業務プロセス制御

---

## 比較

| 観点 | Bedrock | LangChain | LangGraph | Step Functions |
|------|--------|-----------|-----------|----------------|
| 難易度 | 低 | 中 | 高 | 中 |
| 柔軟性 | 低 | 高 | 非常に高い | 高 |
| 状態管理 | 弱い | 弱い | 強い | 強い |
| LLM特化 | ◎ | ◎ | ◎ | △ |
| AWS統合 | ◎ | ○ | ○ | ◎ |

---

## 使い分け

簡単に作る → Bedrock  
柔軟なRAG → LangChain  
エージェント制御 → LangGraph  
業務フロー → Step Functions  

---

## 試験ポイント

RAG → LangChain  
state / graph → LangGraph  
workflow / retry → Step Functions  
簡単なagent → Bedrock  

---

## まとめ

LangChain = LLMアプリ  
LangGraph = エージェント制御  
Step Functions = 業務フロー  
Bedrock = モデル実行  

👉 LLM中心か業務中心かで判断する
