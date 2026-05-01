## 目次
- [Amazon Bedrock Flows](#amazon-bedrock-flows)
- [Bedrock Prompt Management](#bedrock-prompt-management)
- [Bedrock Model Access Policies と SCP](#bedrock-model-access-policies-と-scp)
- [Bedrock AgentCore ツール](#bedrock-agentcore-ツール)
- [Amazon Bedrock Data Automation](#amazon-bedrock-data-automation)
- [Bedrock モデル評価ジョブ（プロンプト評価）](#bedrock-モデル評価ジョブプロンプト評価)
- [Bedrock Agent Trace機能](#bedrock-agent-trace機能)
- [Bedrock Guardrail強制（IAM）](#bedrock-guardrail強制iam)
- [API Gatewayでのノイジーネイバー対策 (Bedrock)](#api-gatewayでのノイジーネイバー対策-bedrock)
- [Bedrock Knowledge Base におけるクエリ分解（Query Decomposition）](#bedrock-knowledge-base-におけるクエリ分解query-decomposition)
- [Bedrock Guardrail 強制（IAM）](#bedrock-guardrail-強制iam)
- [Bedrock Guardrails 分析（trace \& メトリクス）](#bedrock-guardrails-分析trace--メトリクス)
- [Amazon Bedrock モデル呼び出しログ](#amazon-bedrock-モデル呼び出しログ)
- [Amazon Bedrock AgentCore](#amazon-bedrock-agentcore)
- [Bedrock 非同期推論（Async Invocation）](#bedrock-非同期推論async-invocation)
- [Bedrock Knowledge Bases Advanced Parsing](#bedrock-knowledge-bases-advanced-parsing)
- [Bedrock Human Evaluation](#bedrock-human-evaluation)

---

# Amazon Bedrock Flows
https://docs.aws.amazon.com/ja_jp/bedrock/latest/userguide/flows-how-it-works.html

## 正解キーワード
- ステップチェーン（Sequential / Prompt Chaining）
- 条件分岐（Conditional Branching）
- 感情分類（Sentiment Classification）
- テンプレート分岐（Dynamic Prompt Selection）
- ワークフロー依存関係

---

## 解法パターン

### 基本構造
User Input  
→ 要約（Summarization）  
→ 感情判定（Classification）  
→ 条件分岐（if / switch）  
→ 回答生成（Prompt Template）

---

## なぜこれが正解か

- Bedrock Flowsは**複数ステップの依存関係を管理できる**
- 出力を次のステップに渡す**パイプライン構造**が前提
- 条件に応じた**分岐処理（Routing / Branching）**が可能

---

## 試験での判断ポイント

### 出題の特徴
- 「複数ステップ」
- 「結果に応じて処理を変える」
- 「テンプレート切り替え」

👉 この3つが来たら：

→ **Bedrock Flows + 条件分岐**

---

## よくある誤答パターン

- 単一プロンプトで全部処理
→ ❌ 分岐できない / 保守性低い

- Agentを使う
→ ❌ 今回はワークフロー制御が目的（Agent不要）

---

## 試験対策まとめ

- 複数ステップ → Chaining
- 条件による分岐 → Branching
- テンプレ切替 → Bedrock Flows

最重要：
「出力に応じて処理を分岐するなら Bedrock Flows」

# Bedrock Prompt Management
https://docs.aws.amazon.com/ja_jp/bedrock/latest/userguide/prompt-management.html

## 正解キーワード
- Prompt Management
- テンプレート化（Template）
- フォーマット統一
- 共通プロンプト
- 再利用（Reusable Prompt）
- 部門横断共有

---

## 解法パターン

### 問題の特徴
- 出力フォーマットを**必ず統一したい**
- 複数チーム / ワークロードで**共通化したい**
- プロンプトの構造を**一元管理したい**

👉 この条件なら  
→ **Prompt Management**

---

## なぜこれが正解か

- Prompt Managementは**プロンプトテンプレートを一元管理**できる
- 見出しや構造をテンプレートに固定できる
- 全ワークロードで同じテンプレートを使うことで**出力の一貫性を保証**
- バージョン管理により変更も安全に適用可能

---

## 試験での判断ポイント

### こう書いてあれば即選択
- フォーマットを統一したい
- 出力構造を固定したい
- チーム間で共通化したい
- プロンプトを管理したい

👉 これらが出たら  
→ **Prompt Management**

---

## よくある誤答パターン

### コード内でプロンプトをハードコード
→ ❌ 再利用不可 / 管理困難

### AppConfig
→ ❌ 設定管理であってプロンプト構造管理ではない

### Guardrails
→ ❌ 出力制御・安全性であり、フォーマット統一ではない

---

## 試験対策まとめ

- フォーマット統一 → Prompt Management
- プロンプト共通化 → Prompt Management
- 再利用 / バージョン管理 → Prompt Management

最重要：
「プロンプトの構造を統一・管理するなら Prompt Management」

---

# Bedrock Model Access Policies と SCP

## 概要
- Bedrock利用のガバナンスは複数レイヤーで制御する
- Model Access Policies と SCP を組み合わせて統制する

---

## 全体構造

SCP（最上位ガードレール）
↓
IAM（ユーザー/ロール権限）
↓
Model Access Policies（モデル単位制御）
↓
モデル利用

---

## Model Access Policies

### 役割
- Bedrock内でどのモデルを使えるか制御

### 特徴
- モデル単位で許可/拒否
- Bedrockの機能として提供

### 例
- Claudeは許可
- Titanは禁止

---

## SCP（Service Control Policy）

### 役割
- 組織全体の利用上限ルールを定義

### 特徴
- IAMより強い制約
- OU/アカウント単位で適用
- 許可ではなく「上限（ガードレール）」を定義

### 制御例
- Bedrockの利用禁止
- 特定リージョンのみ許可
- 特定APIの利用制限

---

## 重要な関係

- SCPはBedrock利用の可否を決める
- Model Access Policiesはモデル単位の利用を決める

---

## 動作イメージ

1. SCPでBedrock利用が許可されているか確認
2. IAMでユーザーの実行権限を確認
3. Model Access Policiesでモデル利用可否を確認
4. すべてOKなら推論可能

---

## ケース別

### ケース①
SCP：Bedrock禁止
Model Access：Claude許可
→ 利用不可

### ケース②
SCP：Bedrock許可
Model Access：Claude禁止
→ 利用不可

### ケース③
SCP：Bedrock許可
Model Access：Claude許可
→ 利用可能

---

## 試験ポイント

- 「組織全体の統制」→ SCP
- 「モデル単位の制御」→ Model Access Policies
- 「ガバナンス強化」→ 両方併用
- 「コンプライアンス」→ Organizations + Bedrock

---

## よくある誤り

- SCPでモデル単位制御 → NG
- Model Accessだけで十分 → NG
- IAMだけで統制可能 → NG

---

## 一言まとめ

- SCP = 利用の枠を決める
- Model Access Policies = 使えるモデルを決める

---

# Bedrock AgentCore ツール

## 概要
- AgentCore = エージェントに機能（能力）を追加するツール群
- 「何をさせたいか」でツールを選ぶのがポイント

---

## ツール一覧と役割

### 1. Browser Tool
- 役割：外部Webアクセス
- できること：
  - Webページ取得
  - 最新情報の取得
  - 内容の要約
- キーワード：最新情報 / Web / 論文 / ニュース

---

### 2. Code Interpreter
- 役割：コード実行（主にPython）
- できること：
  - データ分析
  - 統計計算
  - グラフ生成
  - ファイル処理（CSV等）
- キーワード：分析 / 計算 / 可視化 / グラフ

---

### 3. Gateway
- 役割：外部API連携の入口
- できること：
  - SageMakerエンドポイント呼び出し
  - ComprehendなどのAPI呼び出し
  - 社内API統合
- キーワード：API連携 / エンドポイント / 外部サービス

---

### 4. Memory
- 役割：長期記憶（コンテキスト保持）
- できること：
  - 会話履歴保存
  - ユーザ状態の保持
  - セッション継続
- キーワード：履歴 / コンテキスト / セッション

---

### 5. Identity
- 役割：認証・認可管理
- できること：
  - ユーザ認証
  - アクセス制御
  - ロール管理
- キーワード：認証 / セキュリティ / 権限

---

## 使い分けまとめ

- Webから情報取得 → Browser Tool
- 計算・分析・グラフ → Code Interpreter
- API連携 → Gateway
- 会話の記憶 → Memory
- 認証・アクセス制御 → Identity

---

## 試験ポイント

- 「最新情報」→ Browser Tool
- 「分析・統計」→ Code Interpreter
- 「API呼び出し」→ Gateway
- 「履歴保持」→ Memory
- 「認証」→ Identity

---

## 一言まとめ

- AgentCore = エージェントに「能力」を追加するツール群

---

# Amazon Bedrock Data Automation 

## 概要
- Bedrock Data Automation = 生成AI向けデータパイプラインを自動化するフルマネージドサービス
- データ取り込み・前処理・強化・連携を一括管理
- 手動ETLや個別オーケストレーションを削減

---

## 主な役割

### 1. データ取り込み（Ingestion）
- S3などからデータを継続的に取り込み
- 新規・更新データを自動検知

---

### 2. データ前処理（Preprocessing）
- クリーニング
- 正規化
- フォーマット統一

---

### 3. データ強化（Enrichment）
- Amazon Comprehend連携
  - 感情分析
  - エンティティ抽出
  - キーフレーズ抽出

---

### 4. ML/GenAI連携
- SageMakerへの学習データ供給
- Foundation Model（Bedrock）への入力データ提供
- 再トレーニングパイプラインと統合

---

### 5. パイプライン自動化
- スケジューリング
- トリガー処理
- ワークフロー管理

---

### 6. 監視・運用
- ログ管理
- モニタリング
- エラーハンドリング

---

## 特徴

- フルマネージド
- スケーラブル
- 低運用負荷
- データ処理の一貫性確保
- 再現性の高いパイプライン

---

## 他リソースとの比較

### 自前構築
- EventBridge
- Lambda
- Glue
- Step Functions

→ 柔軟だが複雑

---

### Data Automation

- 上記を1つに抽象化
- 設計・運用コスト削減

---

## 向いているケース

- 継続的データ取り込み
- MLパイプライン自動化
- ETLの簡素化
- GenAI用データ準備
- CRM / ログ / 文書処理

---

## 向いていないケース

- 単発処理
- 高度なカスタムロジック
- 細かい制御が必要なワークフロー

---

## 試験ポイント

- 「自動化」「継続処理」→ Data Automation
- 「手動ETL削減」→ Data Automation
- 「GenAIパイプライン」→ Data Automation
- 「複数サービス統合」→ Data Automation

---

## 一言まとめ

- Bedrock Data Automation = GenAI向けデータパイプラインのフルマネージド化

---

# Bedrock モデル評価ジョブ（プロンプト評価）

## 概要
- モデル評価ジョブ = LLMの性能を測るための仕組み
- JSONL形式のプロンプトデータセットを入力として使用
- 各プロンプトに対するモデルの出力をスコアリング

---

## 評価の流れ

① 評価用データセットを準備（例：15000件のプロンプト）  
↓  
② 各モデルに同じプロンプトを入力  
↓  
③ モデル出力を生成  
↓  
④ 正解データや評価基準に基づいてスコアリング  
↓  
⑤ モデル性能を比較  

---

## プロンプトデータセット（JSONL）

例：

```json
{"prompt": "この文章を分類してください", "reference": "A"}
{"prompt": "この文章を要約してください", "reference": "B"}
```

- 1行 = 1プロンプト
- 大量データ（例：15000件）で評価精度向上

---

## 何を評価する？

- 正確性（Accuracy）
- 関連性（Relevance）
- 一貫性
- スタイル・トーン
- ハルシネーション

---

## サービス制限（重要）

- 1回の評価ジョブで処理できるプロンプト数に上限あり（例：1000件）

---

## よくあるエラー

- 大量データ（例：15000件）を1回で処理しようとして失敗

---

## 解決方法

データを分割する

例：

15000件  
↓  
1000件ずつ分割  
↓  
複数ジョブで評価  

---

## 試験ポイント

- 「評価データが多すぎる」→ 分割
- 「サービス制限」→ 小さく分けて実行
- 「自動評価」→ Bedrock評価ジョブ

---

## 一言まとめ

- モデル評価 = 「大量のプロンプトを使ったテスト」
- 制限がある場合は「分割して実行」

---

# Bedrock Agent Trace機能

## 概要
- Bedrock Agentのtrace機能は、エージェントの内部処理（思考プロセス）を可視化するための仕組み
- enableTrace = true で有効化
- デバッグ・監視・品質改善に必須

---

## Traceの3つの構成

### ① PreProcessingTrace

#### 役割
- ユーザー入力の解釈
- 意図（Intent）の理解

#### 何が分かる？
- 入力がどう解釈されたか
- タスク分類結果

#### 例
ユーザー入力：  
「この顧客に適用される規制は？」  

↓  
PreProcessingTrace：  
- intent: regulation_lookup  
- entities: customer_type  

---

### ② OrchestrationTrace

#### 役割
- エージェントの推論と行動

#### 何が分かる？
- ナレッジベース検索内容
- Action Groupの呼び出し
- 使用されたパラメータ
- 処理の順序

#### 例
- KB検索クエリ: "顧客 規制 金融"
- 呼び出しAPI: getCustomerContext
- パラメータ: customer_id=123

---

### ③ PostProcessingTrace

#### 役割
- 最終回答の生成

#### 何が分かる？
- 取得情報をどうまとめたか
- 応答生成ロジック

#### 例
- 取得した規制情報を統合
- 最終回答を生成

---

## 使いどころ

### デバッグ
- なぜ誤った回答になったか分析

### ハルシネーション検出
- 不正確な推論箇所を特定

### 品質改善
- プロンプトやツール設計の改善

### 監査・説明責任
- 判断根拠の可視化

---

## 試験ポイント

- 「内部推論を見たい」→ Trace
- 「どのツールを使ったか知りたい」→ OrchestrationTrace
- 「入力解釈を確認したい」→ PreProcessingTrace
- 「出力生成の過程を知りたい」→ PostProcessingTrace

---

## 一言まとめ

- PreProcessing = 入力理解
- Orchestration = 思考と行動（最重要）
- PostProcessing = 出力生成

👉 Trace = エージェントの思考ログ

---

# Bedrock Guardrail強制（IAM）

## 概要
Bedrock Guardrailsは、モデル出力の安全性やコンプライアンスを担保する仕組み

ただし、アプリ側でGuardrailを付け忘れると無効化されるリスクがあるため、
IAMポリシーで強制する必要がある

---

## コア概念

### bedrock:GuardrailIdentifier

- Guardrailがリクエストに含まれているかを判定するIAM条件キー
- Guardrailが無いリクエストを拒否可能
- 特定のGuardrailのみ許可することも可能

---

## できること

### ① Guardrail必須化
- Guardrailなし → 拒否
- Guardrailあり → 許可

### ② バイパス防止
- 開発者が直接APIを叩いても回避できない

### ③ 対象API
- InvokeModel
- Converse
- Streaming系API

---

## メリット

### 最小運用オーバーヘッド
- IAMだけで制御可能
- API GatewayやLambda不要

### 強制力
- アプリケーション側で無効化できない

### セキュリティ
- 中央集権的なガバナンスが可能

---

## 他の方法との比較

### API Gateway + Lambda
- 実現可能だが運用コストが高い

### Secrets Manager
- Guardrail IDを渡すだけで強制できない

### Prompt Router
- ルーティング用途であり、Guardrail強制には不適

---

## 試験ポイント

- Guardrailを必須化したい → IAM + bedrock:GuardrailIdentifier
- バイパスさせたくない → IAM制御
- 最小運用 → IAM一択

---

## まとめ

Guardrail必須化は IAM の bedrock:GuardrailIdentifier で強制する

---

# API Gatewayでのノイジーネイバー対策 (Bedrock)

## 概要
複数アプリが同一のBedrockリソースを共有する場合、
一部のアプリが過剰リクエストを送ることで他アプリに影響する問題（ノイジーネイバー）が発生する

---

## 問題の本質

- バッチ処理が大量リクエストを送信
- リアルタイムアプリがスロットリングされる
- 共有リソースの奪い合い

---

## 解決アプローチ

API Gatewayで「クライアント単位」にトラフィック制御を行う

---

## コア仕組み

### ① API Key
- クライアント識別子
- アプリごとに発行

---

### ② Usage Plan
- API Keyに紐づく制御設定
- スロットリング（レート制限）
- クォータ（総量制限）

---

## アーキテクチャイメージ

バッチアプリ  
 ↓（API Key A）  
API Gateway（Usage Plan A：低レート）  
 ↓  
Lambda  
 ↓  
Bedrock  
  
チャットボット  
 ↓（API Key B）  
API Gateway（Usage Plan B：高レート）  
 ↓  
Lambda  
 ↓  
Bedrock  

---

## ポイント

### ✔ クライアント単位で制御
- エンドポイント分離は必須ではない
- API Key単位で制御できる

---

### ✔ バッチ側を制限
- バッチのリクエストを絞る
- リアルタイム用の帯域を確保

---

### ✔ Bedrockには直接制御機能なし
- フロントで制御する必要あり

---

## よくある誤解

### ❌ エンドポイント分けないとダメ
→ 不要（API Keyで分離可能）

---

### ❌ 完全に遮断できる
→ Usage Planはベストエフォート

---

## 試験ポイント

- ノイジーネイバー問題 → API Gateway
- スロットリング制御 → Usage Plan
- クライアント識別 → API Key

---

## 一言まとめ

API Gateway = クライアント単位でリクエスト量を制御し、共有リソースの公平性を保つ仕組み

---

# Bedrock Knowledge Base におけるクエリ分解（Query Decomposition）

## 概要
クエリ分解とは、ユーザーの複雑な質問を複数のサブクエリに分割し、
それぞれに対して検索を行うことでRAGの精度を向上させる仕組み。

Amazon Bedrock Knowledge Basesでは、この機能を設定で有効化できる。

---

## 一言で

クエリ分解 = 複雑な質問を「検索しやすい複数の質問」に分ける

---

## なぜ必要か

複雑なクエリは：

- 意味が曖昧になる
- 検索精度が低下する
- 重要な情報が埋もれる

---

## 問題例

「糖尿病患者におけるインスリン治療と副作用の関係は？」

---

## クエリ分解後

① 糖尿病患者 インスリン治療  
② インスリン 副作用  

→ それぞれ検索して結果を統合

---

## 効果

### ✔ 検索精度向上

- Recall向上（取りこぼし減少）
- Precision向上（ノイズ減少）

---

### ✔ 意味の希釈防止

- クエリを明確化

---

### ✔ ドメイン特化に強い

- 医療・法律などの専門用語に有効

---

## 動作イメージ

ユーザークエリ  
 ↓  
LLMによるクエリ分解  
 ↓  
複数クエリで検索（Vector DB）  
 ↓  
結果統合  
 ↓  
LLMで最終回答生成  

---

## 誰がやるのか

- 内部的にはLLMが自然言語理解して分解
- Bedrock Knowledge Baseでは設定で有効化可能

---

## 他の手法との違い

### ❌ クエリ抽出
- 不要部分を削る

---

### ❌ クエリ書き換え
- 1つのクエリを改善

---

### ✅ クエリ分解
- 複数のクエリに展開

---

## ユースケース

- 医療質問応答
- 法律ドキュメント検索
- 複雑なビジネス分析

---

## メリット

- 高精度RAG
- 複雑クエリ対応
- マネージドで簡単導入

---

## デメリット

- 検索回数増加（わずかなコスト増）
- 単純クエリでは効果薄

---

## 試験ポイント

以下でクエリ分解：

- 複雑な質問
- 意味の希釈
- 高精度RAG
- ドメイン特化検索

---

## 一言まとめ

クエリ分解 = 複雑な質問を分割して検索精度を高めるRAG最適化手法

---

# Bedrock Guardrail 強制（IAM）

## 概要
Amazon Bedrock の Guardrail を「必須化」するために、
IAM ポリシーでリクエストに GuardrailIdentifier が含まれていない場合に拒否する仕組み

---

## 一言で

Guardrail強制 = Guardrailが付いていないリクエストをIAMで拒否する

---

## 対象API

- bedrock:InvokeModel
- bedrock:InvokeModelWithResponseStream
- bedrock:Converse
- bedrock:ConverseStream

---

## どこに設定する？

❌ Bedrockサービス側ではない  
✅ Bedrockを呼び出す側（IAMロール）

例：
- Lambdaの実行ロール
- ECS / EC2のロール
- ユーザーIAMロール

---

## 動作フロー

アプリ  
 ↓  
Bedrock API呼び出し（GuardrailIdentifier付き？）  
 ↓  
IAMポリシー評価  
 ↓  
✔ 付いている → 許可  
❌ 付いていない → Deny（Bedrockに到達しない）  

---

## ポイント

### ✔ IAMがチェックしているもの

- GuardrailIdentifierが「指定されているか」

---

### ❌ IAMが見ていないもの

- Guardrailの中身
- Guardrailの評価結果

---

## サンプルポリシー
```json
{
  "Effect": "Deny",
  "Action": [
    "bedrock:InvokeModel",
    "bedrock:InvokeModelWithResponseStream",
    "bedrock:Converse",
    "bedrock:ConverseStream"
  ],
  "Resource": "*",
  "Condition": {
    "StringNotEquals": {
      "bedrock:GuardrailIdentifier": "arn:aws:bedrock:region:account-id:guardrail/xxxx"
    }
  }
}
```
---

## なぜIAMでやるのか

- Bedrock側には「Guardrail必須化」設定がない
- 呼び出し制御はIAMで行うのがベストプラクティス

---

## 試験ポイント

以下のキーワードでIAM制御：

- Guardrail強制
- バイパス防止
- InvokeModel / Converse
- 最小運用オーバーヘッド

---

## 一言まとめ

Guardrail強制 = Guardrailを指定しないリクエストをIAMでブロックする仕組み

---

# Bedrock Guardrails 分析（trace & メトリクス）

## 概要
Amazon Bedrock Guardrailsで、なぜコンテンツがブロックされたのかを分析する方法  
trace（詳細ログ）とメトリクス（統計）の2つを組み合わせて分析する  

---

## 一言で

trace = 個別の原因分析  
メトリクス = 全体の傾向分析

---

## ① trace

### 設定

guardrailConfig = {
  "trace": "enabled"
}

---

### 何がわかる？

- どのガードレールルールが発動したか
- なぜブロックされたか（理由）
- どの部分が問題だったか

---

### 用途

- デバッグ
- 誤検知の特定
- チューニング

---

### イメージ

1リクエストごとに：

「この入力は SensitiveInformationPolicy によりブロックされました」
「理由：メールアドレスが検出されました」

---

## ② メトリクス（InvocationsIntervened）

### ディメンション

- ContentPolicy
- TopicPolicy
- SensitiveInformationPolicy

---

### 何がわかる？

- どの種類のガードレールが多く発動しているか

---

### 用途

- 傾向分析
- 誤検知の多いルール特定

---

### イメージ

- 80% が SensitiveInformationPolicy でブロック
→ このルールが厳しすぎるかも

---

## 比較

| 項目 | trace | メトリクス |
|---|---|---|
| 粒度 | 個別 | 集計 |
| 内容 | 詳細理由 | 種類別カウント |
| 用途 | デバッグ | モニタリング |

---

## 使い分け

### ✔ trace
- なぜブロックされたか知りたい
- ルール調整したい

---

### ✔ メトリクス
- 全体傾向を知りたい
- 運用監視したい

---

## 試験ポイント

以下のキーワードで trace：

- なぜブロックされたか
- 詳細分析
- ガードレールの調整

---

以下のキーワードでメトリクス：

- 発動頻度
- 傾向分析
- 監視

---

## 一言まとめ

Guardrail分析 = trace（原因） + メトリクス（傾向）

---

# Amazon Bedrock モデル呼び出しログ

## 概要
Amazon Bedrockは、モデル呼び出し時の詳細なログをAmazon S3に直接出力できる  
コンプライアンス・監査・分析用途で重要  
Bedrockログ = 「誰が・何を・どう呼んで・どう返ったか」を記録する仕組み  

---

## 記録される主な情報

### ① リクエスト情報
- ユーザー入力（プロンプト）
- システムプロンプト
- ツール呼び出し内容（Agent）

---

### ② レスポンス情報
- モデル出力
- トークン数（input / output）

---

### ③ メタデータ
- modelId
- API種別（InvokeModel / Converse）
- タイムスタンプ
- リージョン

---

### ④ 呼び出し元情報
- IAMロール / ユーザー
- AWSアカウントID

---

### ⑤ 推論パラメータ
- temperature
- top_p
- max_tokens
- stop sequences

---

### ⑥ Guardrail情報
- 使用されたGuardrail
- ブロック有無
- trace有効時は詳細理由

---

## 主な用途

### ✔ 監査（コンプライアンス）
- 長期保存（例：7年）
- 操作履歴の追跡

---

### ✔ トラブルシュート
- 出力の再現
- 不正な応答の原因分析

---

### ✔ コスト分析
- トークン使用量の可視化

---

### ✔ セキュリティ
- 誰がどのデータを入力したか追跡

---

## アーキテクチャ

Bedrock  
 ↓  
ログ出力（ネイティブ）  
 ↓  
Amazon S3  
 ↓  
（オブジェクトロックで長期保持）  

---

## なぜS3直接出力が重要か

### ❌ EventBridge + CloudWatch
- カスタム実装必要
- 運用負荷が高い

---

### ✅ Bedrockネイティブログ
- フルマネージド
- 低運用コスト
- 直接S3保存

---

## 注意点

- PIIや機密情報もログに含まれる可能性あり
- アクセス制御・マスキング設計が必要

---

## 試験ポイント

以下のキーワードでBedrockログ：

- 長期保存（7年）
- コンプライアンス
- 監査ログ
- 低運用オーバーヘッド

---

## 一言まとめ

Bedrockログ = モデル呼び出しの全情報をS3に記録する監査・分析基盤

---

# Amazon Bedrock AgentCore

## 概要
Amazon Bedrock AgentCore は、PythonコードをそのままAPIとして実行できるマネージドランタイム  
インフラ・サーバ・コンテナ管理を抽象化し、開発者はロジック実装に集中できる  

---

## 一言で

AgentCore = 「コードを書くだけでAPIとして動く仕組み」

---

## できること

### ✔ HTTPサーバ自動化
@app.entrypoint デコレータで：

- HTTPサーバ起動
- リクエスト受信
- ルーティング設定

を自動化

---

### ✔ APIルーティング不要

- /invoke などのエンドポイント定義不要
- リクエストハンドリングを自動化

---

### ✔ コンテナ化の自動化（starter toolkit）

- Dockerfile生成
- 依存関係パッケージング
- ビルド & デプロイ自動化

---

### ✔ 実行基盤の抽象化

- スケーリング自動
- 短時間応答・長時間ストリーミング対応
- Bedrock Runtimeで実行

---

## 開発者がやること

- Pythonコードを書く
- ビジネスロジック設計
- 必要なライブラリ指定

---

## 開発者がやらなくていいこと

❌ Webサーバ構築  
❌ APIルーティング設定  
❌ コンテナ管理  
❌ ヘルスチェック実装  

---

## アーキテクチャイメージ

Pythonコード  
 ↓  
AgentCore SDK（entrypoint）  
 ↓  
自動API化  
 ↓  
AgentCore Runtime（マネージド実行）  

---

## メリット

- 開発スピード向上
- インフラ管理不要
- サーバレス的な開発体験
- 長時間処理（ストリーミング）も簡単対応

---

## 試験ポイント

以下のキーワードでAgentCore：

- サーバ設定不要
- APIルーティング不要
- コンテナ管理不要
- 開発者の負担軽減
- 長時間ストリーミング処理

---

## 他サービスとの違い

| サービス | 役割 |
|---|---|
| AgentCore | API化 + 実行基盤 |
| Lambda | 関数実行 |
| API Gateway | ルーティング |
| ECS/EKS | コンテナ管理 |

---

## 一言まとめ

AgentCore = インフラを意識せずにAgentアプリをデプロイ・実行できるマネージド基盤

---

# Bedrock 非同期推論（Async Invocation）

## 概要
Amazon Bedrockの非同期推論（StartAsyncInvoke）は、  
長時間かかる生成処理をHTTP接続から切り離して実行する仕組み  

---

## 一言で

非同期推論 = 「受付だけ返して、結果は後で取得」

---

## 同期 vs 非同期

### 同期推論（InvokeModel）

- リクエスト送信
- 処理完了まで接続を維持
- 結果をその場で返却

問題：
- 長時間処理でタイムアウト（例：ALB 60秒制限）

---

### 非同期推論（StartAsyncInvoke）

- リクエスト送信
- invocationArn を即時返却
- 接続終了
- 裏で処理継続
- 結果はS3に保存

---

## フロー

1. StartAsyncInvoke を呼び出し
2. invocationArn を取得
3. HTTPセッション終了
4. Bedrockがバックグラウンド処理
5. 結果をS3に出力
6. GetAsyncInvoke で状態確認（polling）
7. 完了後、S3から結果取得

---

## なぜ必要か

- 動画生成など数分かかる処理に対応
- API Gateway / ALB のタイムアウト回避
- フロントエンドの待ち時間削減

---

## ポイント

- セッションを維持しない（ここ重要）
- 状態管理は invocationArn で行う
- 結果は直接レスポンスではなくS3に出力

---

## 試験ポイント

以下の条件で非同期推論：

- 長時間処理（数分以上）
- タイムアウト制約あり（ALB / API Gateway）
- リアルタイム応答不要

---

## よくある誤り

❌ InvokeModel + クライアントポーリング  
→ 同期処理なのでタイムアウト回避できない  

---

## 一言まとめ

Bedrock非同期推論 = 長時間生成をHTTP接続から切り離して処理する仕組み

---

# Bedrock Knowledge Bases Advanced Parsing

## 概要
Advanced Parsing は、Amazon Bedrock Knowledge Bases のデータ取り込み時に、  
Foundation Model（FM）を利用してドキュメントの構造を理解する機能  

---

## 一言で

Advanced Parsing = 「ドキュメント構造をAIで理解してからチャンク化する」  

---

## 従来の取り込みの問題

### 通常のフロー
PDF → テキスト抽出 → 平坦化  

問題：
- 表構造が壊れる
- カラム関係が消える
- 意味が失われる

---

## Advanced Parsing の動き

PDF → FMが構造解析 → 意味を保ったチャンク生成  

理解できるもの：
- テーブル（列・行の関係）
- レイアウト
- セクション構造

---

## 効果

- RAG検索精度向上
- 誤回答（ハルシネーション）削減
- 表データの正確な理解

---

## 使用方法

Knowledge Base の Data Source 設定で：

- Advanced parsing を有効化
- 使用するFoundation Modelを選択

---

## 試験ポイント

以下のキーワードが出たら：

- PDF
- tables（表）
- レイアウト崩壊
- 構造が失われる
- least effort

→ Advanced parsing を選択  

---

## 他手法との比較

### ❌ Semantic Chunking
- テキストの意味で分割
- 構造は復元できない

### ❌ Parent-Child Chunking
- 文脈を保持する
- 構造が壊れていると意味なし

### ❌ Textract + Lambda
- 正確だが実装コスト高

---

## 一言まとめ

Advanced Parsing = 「壊れた構造をAIで復元してからEmbeddingする」

---

# Bedrock Human Evaluation

## 概要
Amazon Bedrock Model Evaluation は、人間による主観的評価を実施できる機能  
創造性・トーン・ペルソナ適合など、自動指標では測れない品質を評価可能  

---

## 一言で

Human Evaluation = 人間がモデル出力を評価する仕組み

---

## 主要機能

### ✔ Human Evaluation
- 人間が出力を直接レビュー
- 主観的品質（創造性・自然さ）を評価

---

### ✔ Model Comparison（重要）
- BaseモデルとFine-tunedモデルを並べて比較
- どちらが優れているかを人間が判断

---

### ✔ Private Workforce
- 社内メンバーのみで評価可能
- 専門家（例：ナラティブデザイナー）によるレビュー

---

### ✔ カスタム評価指標
- Likertスケール（例：1〜5）
- 指標例：
  - Creativity
  - Persona Alignment
  - Tone

---

## なぜ必要か

自動評価の限界：

- ROUGE / BLEU / BERTScore → 類似度評価
- 創造性や自然さは測れない

---

## 自動評価との違い

| 評価方法 | 特徴 |
|---|---|
| 自動評価 | 客観的・高速・スケーラブル |
| 人間評価 | 主観的・高品質・コスト高 |

---

## 試験ポイント

以下のキーワードでHuman Evaluation：

- 創造性（Creativity）
- トーン（Tone）
- ペルソナ（Persona）
- 人間の主観
- UX品質

---

## 逆に自動評価を選ぶケース

- 正解データあり
- 分類・翻訳など精度評価
- 類似度測定

---

## 一言まとめ

Human Evaluation = 数値で測れない品質を人間が評価する仕組み
