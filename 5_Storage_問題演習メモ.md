## 目次
- [S3 Glacier / Retrieval オプションまとめ](#s3-glacier--retrieval-オプションまとめ)
- [Lake Formation LF-Tag](#lake-formation-lf-tag)

---

# S3 Glacier / Retrieval オプションまとめ

## 概要
- 低頻度アクセスデータを低コストで保管するためのS3アーカイブ系ストレージ
- 取り出し速度とコストのトレードオフを設計する

---

## ストレージクラス

### S3 Glacier Instant Retrieval
- 即時アクセス（ミリ秒）
- Restore不要
- 低頻度だが即時参照が必要なデータ向け

### S3 Glacier Flexible Retrieval（旧 Glacier）
- 低コスト
- 取得時にRestoreが必要
- 複数のリトリーブ方式を選択可能

### S3 Glacier Deep Archive
- 最安
- 取得に長時間（12時間以上）
- ほぼ参照しないデータ向け

---

## Retrieval（取り出し）方式

※ Flexible Retrievalのみ対象

### Expedited Retrieval
- 数分で取得
- 高コスト
- 緊急時・即時利用向け

### Standard Retrieval
- 数時間
- バランス型

### Bulk Retrieval
- 半日〜
- 低コスト

---

## Deep Archiveの制約
- Expeditedは使用不可
- Standardでも12時間程度
- Bulkはさらに遅い（最大48時間）

---

## 試験での読み替えルール

- 「S3 Glacier」 → Glacier Flexible Retrieval を指す
- 「Expedited Retrieval」 → Flexible Retrievalの高速取得方法
- 「Instant Retrieval」 → ストレージクラス（即時アクセス）

---

## 使い分け

### 即時アクセスが必要
- Glacier Instant Retrieval

### 普段使わないがすぐ取り出したい
- Glacier Flexible Retrieval + Expedited

### とにかく安く、滅多に使わない
- Glacier Deep Archive

---

## ライフサイクル設計

- アクセス頻度に応じて自動移行
- 例
  - Standard → Intelligent-Tiering → Glacier → Deep Archive

---

## 試験ポイント

- コストだけでなく「取得時間」を見る
- ExpeditedはFlexible Retrieval専用
- Deep Archiveは高速取得不可
- アクセスパターン不明 → Intelligent-Tiering

---

## よくある誤り

- Deep Archiveを選択して取得時間を無視
- ExpeditedとInstantを混同
- Glacierを単一のストレージクラスと誤解

---

## 一言まとめ

- Expedited = 取り出し方法
- Instant = ストレージクラス
- Glacier（問題文） = Flexible Retrieval

---

# Lake Formation LF-Tag

## 概要
LF-Tagは、AWS Lake Formationで使用される「属性ベースアクセス制御（ABAC）」の仕組み。
データやユーザーにタグを付与し、それを元にアクセス制御を行う。

---

## 一言で

LF-Tag = データにラベルを付けてアクセス制御する仕組み

---

## なぜ必要か

従来の問題：

- ユーザーごとに権限設定 → 管理が破綻
- テーブル単位の制御 → 粗すぎる

---

## 解決

タグベースで一括管理

---

## 基本構造

### ① LF-Tag定義

例：

- region = JP / US
- department = Finance / Sales
- sensitivity = PII / Public

---

### ② データにタグ付与

例：

テーブルA  
→ region=JP  
→ sensitivity=PII  

---

### ③ ユーザー（ロール）にタグ付与

例：

ユーザーX  
→ region=JP  

---

### ④ アクセス制御

条件一致 → アクセス許可

---

## 動作イメージ

ユーザー：
- region = JP

データ：
- region = JP → OK
- region = US → NG

---

## メリット

### ✔ スケーラブル

- 新しいデータ追加 → タグ付けだけ
- 権限変更不要

---

### ✔ 柔軟

- 複数条件で制御可能

例：
region=JP AND department=Finance

---

### ✔ 一元管理

- ポリシーを中央で管理

---

## 従来方式との比較

### ❌ IAMベース

- ユーザー単位管理
- 爆発的に増える

---

### ✅ LF-Tag

- 属性ベース管理
- シンプル＆拡張性高い

---

## PII制御との関係

sensitivity=PII のタグを付けて、

PIIアクセス権のあるユーザーだけ許可

---

## 列レベルとの組み合わせ

- カラム単位でタグ付与可能
- PIIカラムだけ制御できる

---

## 試験ポイント

以下が出たらLF-Tag：

- スケーラブルなアクセス制御
- 属性ベース制御（ABAC）
- 複数条件でアクセス制御
- データレイクの権限管理

---

## 一言まとめ

LF-Tag = タグベースでスケーラブルにデータアクセス制御する仕組み
