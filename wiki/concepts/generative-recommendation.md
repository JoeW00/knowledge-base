---
title: 生成式推薦 (Generative Recommendation)
tags: [recommender-system, generative-ai, llm, paradigm-shift]
created: 2026-04-09
updated: 2026-04-09
---

# 生成式推薦 (Generative Recommendation)

生成式推薦是推薦系統的新興範式，將推薦問題從「在候選集中排序打分」重新定義為「直接生成推薦結果」。傳統判別式推薦學習條件機率 P(y|x)（給定使用者特徵預測評分），生成式推薦則學習聯合分布 P(x,y)，意味著模型同時理解輸入與輸出的生成過程。

## 從判別到生成的典範轉移

傳統推薦系統的運作模式是：給定一組候選物品，為每個物品計算評分 f(u,i)，然後排序取 Top-K。這依賴固定的候選集、任務專用的模型架構、以及精心設計的特徵工程。生成式推薦打破了這些限制——模型可以直接生成物品 ID、自然語言描述、甚至圖像內容，不受預定義候選集的約束。

這個轉變主要由三股力量驅動：LLM 帶來的世界知識與自然語言理解能力、[[scaling-law]] 在推薦領域的驗證（更大的模型與更多的資料持續帶來效能提升）、以及擴散模型在生成品質上的突破。

## 三條技術路線

**LLM-based 生成式推薦**是目前最活躍的方向，分為三個層次：(1) 直接使用預訓練 LLM 的提示工程方法（零樣本或少樣本）；(2) 將 LLM 與推薦目標對齊的微調方法（SFT、RLHF、偏好最佳化）；(3) 訓練目標與推理策略的設計（排序損失、對比學習、受限解碼）。

**[[large-recommendation-model]]**（LRM）是另一條路線，不借用語言模型而是從頭設計專為使用者行為資料最佳化的大規模架構（如 Meta 的 HSTU），驗證了推薦領域自身的 scaling law。

**[[diffusion-recommendation]]** 利用擴散模型的去噪過程來建模使用者偏好的機率分布，特別擅長捕捉偏好的多樣性與不確定性。

## 五大關鍵優勢

1. **世界知識整合**：LLM 預訓練獲得的廣泛知識可直接用於推薦，緩解冷啟動問題
2. **自然語言理解**：能處理文本形式的使用者偏好描述與物品屬性
3. **推理能力**：支援多步推理的 [[explainable-recommendation]]
4. **Scaling law**：模型效能隨規模持續提升，突破判別式模型的效能天花板
5. **創造性生成**：能產出新的推薦內容（文案、圖像），而非僅從既有物品中選擇

## Related

- [[large-recommendation-model]]
- [[diffusion-recommendation]]
- [[conversational-recommendation]]
- [[explainable-recommendation]]
- [[foundation-model]]
- [[scaling-law]]
- [[generative-recommendation-survey-summary]]
