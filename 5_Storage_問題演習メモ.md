## 目次
- [S3 Glacier / Retrieval オプションまとめ](#s3-glacier--retrieval-オプションまとめ)

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
