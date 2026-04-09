---
title: "KV Cache"
tags: [inference, transformer, optimization]
created: 2026-04-09
updated: 2026-04-09
---

## KV Cache

KV Cache（Key-Value Cache）是 Transformer 模型在推理階段用於儲存先前 token 的 key 與 value 向量的快取機制，目的是避免自迴歸解碼時的重複計算。

### 為何需要 KV Cache

Transformer 的自迴歸解碼機制要求每生成一個新 token 時，attention 層都必須參考所有先前 token 的 key 和 value 向量。若不使用快取，每生成一個 token 都需要重新計算整個序列的 key-value，導致計算量隨序列長度呈二次方增長。KV cache 透過儲存已計算過的 key-value 向量，將每步解碼的計算量降為常數級別，是現代 LLM 推理的基礎設施。

### 記憶體佔用

KV cache 的記憶體需求與模型層數、注意力頭數、隱藏維度、序列長度以及批次大小成正比。以一個 70B 參數的模型為例，處理數千 token 的上下文時，KV cache 可能佔用數十 GB 的 GPU 記憶體。這意味著在長上下文場景下（如 128K token），KV cache 的記憶體需求甚至可能超過模型權重本身，成為推理的核心瓶頸。

### 最佳化技術

為降低 KV cache 的記憶體負擔，業界發展出多種最佳化方案。**Multi-Query Attention（MQA）** 讓所有 attention head 共享同一組 key-value，大幅減少快取大小。**Grouped-Query Attention（GQA）** 則在 MQA 與標準 multi-head attention 之間取得平衡，將 attention head 分組共享 key-value。**Cross-layer attention** 嘗試跨層共享 KV cache 以進一步壓縮記憶體。**PagedAttention**（由 vLLM 提出）借鑑作業系統的虛擬記憶體分頁概念，以非連續的記憶體區塊儲存 KV cache，解決了記憶體碎片化問題並提高利用率。

### 與上下文長度的關係

KV cache 的大小與上下文長度成線性關係，這使得支援長上下文（如 100K 以上 token）的模型面臨嚴峻的記憶體挑戰。這也是為何 [[prompt-caching]] 與高效的 KV cache 管理策略在長上下文應用場景中格外重要。

## Related

- [[inference-optimization]]
- [[continuous-batching]]
- [[prompt-caching]]
- [[speculative-decoding]]
