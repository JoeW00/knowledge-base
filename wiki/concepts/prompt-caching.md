---
title: "Prompt Caching"
tags: [inference, optimization, cost]
created: 2026-04-09
updated: 2026-04-09
---

## Prompt Caching

Prompt Caching 是一種在推理過程中快取提示詞中重複片段的技術，透過重用已計算的 [[kv-cache]] 來避免重複處理相同的 token 序列，從而降低成本與延遲。

### 適用場景

Prompt caching 在以下場景中效果最為顯著。**長 system prompt**：當應用程式使用固定且冗長的系統提示詞時（如數千 token 的指令集），每次請求都重新處理相同的前綴是巨大的浪費。**多輪對話共享前綴**：對話歷史中前幾輪的內容在每次請求中重複出現，快取這些共享前綴能大幅節省運算。**RAG 管線固定上下文**：在 RAG 架構中，相同的檢索文件可能被多次引用，快取這些固定上下文段落可避免重複計算。

### 成本與延遲效益

根據實際部署數據，prompt caching 最高可節省約 90% 的推理成本，延遲降低幅度可達 75%。具體效益取決於快取命中率，而命中率又取決於請求之間共享前綴的比例。對於 system prompt 佔請求總長度大部分的應用場景，效益尤其可觀。

### Prefix Caching 實作原理

Prefix caching 是 prompt caching 最主要的實作方式。其原理是在首次處理某段 token 序列時，將計算產生的 KV cache 儲存下來，並以 token 序列的 hash 值作為索引。後續請求若包含相同的前綴序列，系統直接從快取載入 KV cache 而非重新計算，僅需處理前綴之後的新增 token。這要求快取的 token 序列必須從第一個 token 開始完全匹配，中間不能有任何差異。

### API 提供商支援

主要 AI API 提供商如 Anthropic、OpenAI、Google 等均已支援不同形式的 prompt caching，部分為自動啟用，部分需要開發者顯式標記可快取的前綴區段。各家的實作細節與計費方式有所不同。

### 與 Semantic Caching 的區別

Prompt caching 基於精確的 token 序列匹配，安全性高但命中條件嚴格。[[semantic-caching]] 則基於語義相似度進行匹配，命中率更高但存在語義相近卻答案不同的風險。兩者在架構中可互補使用。

## Related

- [[inference-optimization]]
- [[kv-cache]]
- [[semantic-caching]]
- [[continuous-batching]]
