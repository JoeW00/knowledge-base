---
title: "Chinchilla Scaling Law：Token 數 ≈ 資料量？參數量 ≈ 模型設計？"
question: "Chinchilla 建議訓練 token 數約為參數量的 20 倍，token 數就是指訓練的資料量嗎？參數量是與模型設計有關嗎？"
created: 2026-04-09
---

## 簡短回答

你的理解方向正確，但需要一些精確化：

- **Token 數 ≈ 資料量**：大致正確，但有重要的細微差異
- **參數量 ≈ 模型設計**：正確，參數量由架構決策決定

以下展開說明。

---

## Token 數與「資料量」的關係

### Wiki 已有的觀點

根據 [[scaling-law]]，訓練 token 數是衡量模型規模的三大指標之一，「即模型在訓練過程中處理的 token 總量，反映了模型接觸的資料規模」。

根據 [[tokenization]]，token 是模型實際處理的最小單位，「可以是一個完整的單詞、一個子詞片段，甚至是單一字元」。

### 綜合推論

說「token 數就是資料量」在直覺上是對的，但嚴格來說兩者有三個層次的差異：

**1. Token ≠ 字詞，更不等於檔案大小**

同一段文字被不同 tokenizer 處理後，產生的 token 數量不同。例如 "ChatGPT is amazing" 可能被拆成 4 個 token，也可能是 5 個。而根據 [[tokenization]] 的記載，多語言場景下差異更大——中文同樣語義的內容可能需要比英文多出數倍的 token。所以「1 兆 token」並不等於「1 兆個英文單字」，更不直接對應特定的 GB 數。

**2. Token 數可包含重複訓練（multi-epoch）**

假設你有 500 億 token 的語料，但訓練時重複跑了 2 輪（2 epochs），模型實際處理的 token 數為 1000 億。Chinchilla 的「20 倍」指的是模型實際處理的 token 總量（含重複），不是獨立資料的大小。不過，在大模型訓練中通常只跑 1 epoch（因為資料足夠多），此時 token 數確實近似獨立資料量。

**3. 粗略換算**

作為參考量級：英文文本中，1 個 token 大約是 0.75 個英文單詞，或約 4 個字元。所以 1 兆（1T）token ≈ 7500 億英文單詞 ≈ 約 4 TB 原始文字。

> **結論**：說「token 數反映訓練資料量」是合理的直覺理解，但精確來說，它是「模型在訓練中實際處理的 token 總數」，受 tokenizer 設計和訓練輪次影響。

---

## 參數量與「模型設計」的關係

### Wiki 已有的觀點

根據 [[scaling-law]]，參數量是「模型中可學習權重的數量」。根據 [[transformer-architecture]]，模型架構包含 encoder/decoder 結構、attention head 數量、層數、隱藏層維度等設計決策。

### 綜合推論

你說「參數量與模型設計有關」完全正確。更具體地說：

**參數量由以下架構決策共同決定：**

| 設計參數 | 影響 |
|----------|------|
| 層數（depth） | 層越多，參數越多 |
| 隱藏層維度（hidden size） | 維度越大，每層參數越多 |
| Attention head 數量 | 影響注意力層的參數量 |
| Vocabulary 大小 | 影響 embedding 層參數量 |
| 架構類型 | Decoder-only vs. Encoder-Decoder |

例如，GPT-3 是 1750 億參數，其設計選擇為 96 層、隱藏維度 12288、96 個 attention head。若將層數減半，參數量大致也減半。所以參數量是模型設計者主動選擇的結果，不是訓練過程中「長出來」的。

---

## Chinchilla 20:1 的實際意義

把兩者合在一起看 Chinchilla 的建議：

> 若你設計了一個 700 億參數的模型，最佳訓練資料量約為 1.4 兆 token。

這意味著：
- **資料不夠？** 模型「訓練不足」，浪費了參數容量
- **模型太大？** 如 [[scaling-law]] 記載，Chinchilla（700B token, 70B 參數）在多項基準上超越了 Gopher（2800 億參數但訓練不足），因為後者沒有遵守 20:1 比例
- **資料太多但模型太小？** 模型無法吸收更多知識，邊際效益遞減

不過要注意，20:1 是「計算最佳化」的比例——在固定計算預算下的最佳配比。實務上，許多團隊選擇「過度訓練」較小的模型（如 Llama 系列），因為較小模型的推理成本更低，長期部署反而更經濟。

---

## 參考文章

- [[scaling-law]] — Chinchilla scaling law 的完整說明
- [[tokenization]] — Token 的定義與多語言效率差異
- [[foundation-model]] — 基礎模型的適配方法與 MaaS 模式
- [[transformer-architecture]] — 影響參數量的架構設計決策
- [[ch02-understanding-foundation-models-summary]] — 第02章摘要：模型規模衡量與後訓練
