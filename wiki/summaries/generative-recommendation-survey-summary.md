---
title: 生成式推薦系統綜述：資料、模型與任務
source: raw/papers/2510.27157v1.pdf
tags: [recommender-system, generative-ai, llm, diffusion-model, survey]
created: 2026-04-09
updated: 2026-04-09
---

# 生成式推薦系統綜述：資料、模型與任務

Hou et al.（2025）全面調查推薦系統從判別式範式（學習評分函數 f(u,i)）到生成式範式（將推薦重新定義為生成任務，學習聯合分布 P(x,y)）的典範轉移。論文以「資料—模型—任務」三軸框架組織超過 200 篇相關文獻，涵蓋 LLM、大型推薦模型（LRM）與擴散模型三條技術路線。

生成式推薦的核心轉變在於：傳統系統從固定候選集中排序打分，生成式系統則直接**產生**推薦內容——可以是物品 ID、自然語言描述、甚至圖像與影片。這打破了推薦系統長期依賴候選集、任務專用架構、固定評分函數的限制。

## 關鍵論點

1. **資料層的 LLM 賦能**：LLM 在資料生成（內容增強、行為模擬、結構增強）與資料統一（跨域、多任務、多模態）兩方面重塑推薦系統的資料基礎，有效緩解冷啟動與資料稀疏問題。
2. **三條模型路線並行發展**：[[generative-recommendation]] 的模型層分為 LLM-based（提示/對齊/微調）、[[large-recommendation-model]]（如 Meta 的 HSTU，驗證 scaling law 適用於推薦領域）、以及 [[diffusion-recommendation]]（利用去噪過程建模使用者偏好分布）。
3. **任務層的質變**：生成式範式不僅改善 Top-K 推薦，更開啟了全新任務維度——[[conversational-recommendation]]（多輪對話式推薦）、[[explainable-recommendation]]（生成推理鏈與解釋）、以及個人化內容生成（直接產出文本、圖像等推薦內容）。
4. **五大優勢**：世界知識整合、自然語言理解、推理能力、scaling law、創造性生成——這些是傳統判別式模型無法企及的能力。
5. **開放挑戰**：資料面的魯棒性與對抗攻擊；模型面的偏差、公平性與推理效率；部署面的即時性、多場景泛化與評估基準不足。

## 原文路徑

`raw/papers/2510.27157v1.pdf`

## Related

- [[generative-recommendation]]
- [[large-recommendation-model]]
- [[diffusion-recommendation]]
- [[conversational-recommendation]]
- [[explainable-recommendation]]
- [[foundation-model]]
- [[scaling-law]]
- [[rag]]
