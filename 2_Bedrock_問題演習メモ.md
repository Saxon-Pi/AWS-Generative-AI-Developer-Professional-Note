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
