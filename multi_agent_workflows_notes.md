# Multi-Agent Workflows（Agentic AI）

## 概要
Multi-Agent Workflows（Agentic AI）は、複数のエージェント（LLMベースの処理単位）を組み合わせてタスクを実行するアーキテクチャ。タスク分解・並列処理・役割分担により、複雑な問題を効率的に解決する。

---

## 本質
- 単一LLMではなく複数エージェントで処理
- 各エージェントが専門的な役割を担当
- Orchestratorが全体制御を行う

---

## 主なパターン

### ① Parallelization（並列化）

#### 概要
複数エージェントが同時に異なるタスクを処理する。

#### 目的
- 処理速度向上
- 多角的な分析

#### 構成
User Query  
→ Agent A（金融分析）  
→ Agent B（リスク分析）  
→ Agent C（市場分析）  
→ Orchestrator  
→ 統合結果  

---

### ② Majority Voting（多数決）

#### 概要
同一タスクを複数エージェントで実行し、最も多い結果を採用する手法。

#### 目的
- LLMの出力ばらつき（非決定性）を低減
- 安定性向上

#### 例
- Agent1 → A  
- Agent2 → B  
- Agent3 → A  
→ Final Answer = A  

---

### ③ Routing（ルーティング）

#### 概要
ユーザーの入力内容に応じて、最適なエージェントにタスクを振り分ける。

#### 構成
User Query  
→ Router Agent  
→ 適切なエージェント（Finance / Legal / Support など）

#### 目的
- 専門性の最大化
- 不要な処理の削減

---

### ④ Sequential（直列処理 / Prompt Chaining）

#### 概要
エージェントをパイプラインとして順番に実行する。

#### フロー
User Query  
→ Agent1（情報取得）  
→ Agent2（要約）  
→ Agent3（最終生成）

#### 特徴
- 段階的処理
- 精度向上

---

## Orchestratorの役割（重要）

- タスク分解
- エージェント選択
- 実行順序制御
- 結果統合

---

## メリット

- 複雑タスクへの対応力向上
- スケーラビリティ
- モジュール化（再利用性）

---

## デメリット（試験対策）

- システム設計が複雑
- レイテンシ増加（特にSequential）
- コスト増加（複数推論）

---

## AWSでの実装

- Bedrock Agents：エージェント実行・ツール連携
- Bedrock Flows：ワークフロー構築（ノードベース）
- Lambda / Step Functions：オーケストレーション補助

---

## 試験で問われやすい観点

- Parallel / Routing / Sequential の違い
- Orchestratorの役割
- 単一LLMとの比較（複雑タスク対応）
- 多数決による安定化（Majority Voting）

---

## 試験対策まとめ

- Parallel：並列処理（速度・多角分析）
- Routing：適切なエージェント選択
- Sequential：段階処理（精度向上）
- Majority Voting：出力安定化

最重要：
「Orchestratorが全体制御を担う」
