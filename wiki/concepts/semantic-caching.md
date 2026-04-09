---
title: "Semantic Caching"
tags: [architecture, optimization, caching]
created: 2026-04-09
updated: 2026-04-09
---

## Semantic Caching

Semantic Caching（語義快取）是一種基於語義相似度而非精確字串匹配來命中快取的技術，旨在提高 AI 應用中快取的命中率，從而降低重複查詢的推理成本與延遲。

### 與精確快取的對比

傳統精確快取（exact caching）要求查詢文字完全一致才能命中，這在自然語言場景中極度受限——同一個問題可能有無數種表述方式。例如「台灣的首都是哪裡？」與「請問台灣首都是什麼？」語義完全相同，但精確快取無法將它們匹配。精確快取的優勢是安全性高，命中時返回的結果必定正確；劣勢是命中率極低。Semantic caching 正是為了解決這個命中率問題而誕生。

### 實作方式

Semantic caching 的典型實作流程為：將每個查詢通過 embedding 模型轉換為向量表示，存入向量資料庫。新查詢進入時，同樣轉為 embedding，在向量資料庫中搜尋最相似的歷史查詢。若相似度超過預設閾值，則直接返回該歷史查詢的快取回應，無需呼叫 LLM。閾值的設定是關鍵參數——設得太低會增加錯誤命中，太高則快取形同虛設。

### 風險與挑戰

Semantic caching 最大的風險在於語義相近但正確答案不同的情況。例如「2024 年美國總統是誰？」與「2028 年美國總統是誰？」在 embedding 空間中距離很近，但答案截然不同。時間敏感的查詢、包含特定數值的查詢、以及需要個人化回應的查詢，都不適合使用 semantic caching。這類邊界情況需要透過額外的規則過濾或更精細的相似度模型來處理。

### 適用場景

Semantic caching 最適合用於查詢模式穩定且答案變化不大的場景，例如 FAQ 系統、產品知識查詢、客服機器人中的常見問題等。在這些場景中，大量使用者會以不同方式問相同的問題，semantic caching 能將命中率從個位數百分比提升至 30-50%。

### 快取失效策略

有效的快取失效機制對 semantic caching 至關重要。常見策略包括 TTL（time-to-live）過期機制、基於底層知識更新的主動失效、以及定期清理命中率低的快取條目。與 [[prompt-caching]] 不同，semantic caching 位於應用層而非推理引擎層，因此快取管理的職責落在應用開發者身上。

## Related

- [[prompt-caching]]
- [[ai-gateway]]
- [[model-routing]]
- [[inference-optimization]]
