# Bedrock Data Automation

## 概要
Amazon Bedrock Data Automationは、PDF・画像・メールなどの非構造データから情報を抽出し、構造化データへ変換するサービス。LLMとDocument AIを組み合わせてデータ抽出を自動化する。

---

## 本質
- 非構造データ → 構造データへの変換
- 情報抽出（Information Extraction）に特化
- LLMを用いた柔軟なスキーマ対応

---

## 対象データ

- PDF
- 画像（スキャン・帳票）
- メール
- ドキュメント

---

## 出力例

```
{
  "invoice_number": "12345",
  "date": "2024-05-01",
  "total": 1200
}
```

---

## Blueprint

### 概要
抽出したいデータ構造（スキーマ）を定義する仕組み。

---

### 例：Invoice Blueprint

```
{
  "invoice_number": string,
  "date": date,
  "vendor": string,
  "items": [
    {
      "name": string,
      "price": number
    }
  ],
  "total": number
}
```

---

### 動作

ドキュメント  
→ LLMが内容理解  
→ Blueprintにマッピング  
→ 構造化データ生成  

---

## 重要ポイント

- Blueprint = 出力フォーマット定義
- モデルはスキーマに従って情報を抽出
- 固定ルールではなくLLMによる柔軟抽出

---

## ユースケース

- 請求書処理（Invoice processing）
- 契約書解析
- フォームデータ抽出
- メール情報抽出

---

## 従来手法との違い

| 観点 | 従来OCR/ルールベース | Bedrock Data Automation |
|------|----------------------|--------------------------|
| 柔軟性 | 低い | 高い |
| スキーマ変更 | 困難 | 容易（Blueprint変更） |
| 文脈理解 | 弱い | 強い（LLM） |
| メンテナンス | 高コスト | 低コスト |

---

## 試験で問われやすい観点

- 非構造 → 構造データ変換サービス
- Blueprint = スキーマ定義
- LLMベースの情報抽出
- OCR単体ではなく意味理解を伴う

---

## Bedrock内での位置づけ

- Knowledge Bases：検索（RAG）
- Data Automation：構造化（Extraction）

→ 役割が異なるため混同に注意

---

## 試験対策まとめ

- Data Automation = 情報抽出サービス
- Blueprint = 出力構造定義
- 非構造データ処理が主目的

最重要：
「非構造データを構造データに変換する仕組み」
