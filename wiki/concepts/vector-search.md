---
title: Vector Search
tags: [retrieval, rag, infrastructure]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Vector search 是一種基於嵌入向量（embedding）的檢索技術，透過將文本轉換為高維向量空間中的點，再利用近似最近鄰（Approximate Nearest Neighbor, ANN）演算法找出語義上最相似的結果。與傳統的關鍵字檢索不同，vector search 能捕捉語義層面的相似性——即使查詢與文件使用完全不同的措辭，只要語義相近就能成功匹配。這使其成為 [[rag]] 系統中不可或缺的核心元件。

## ANN 演算法與索引類型

精確的最近鄰搜尋在高維空間中計算成本極高，因此實務上幾乎都採用 ANN 演算法，以少量精度損失換取數量級的速度提升。主流索引類型包括：FAISS IVF（Inverted File Index）將向量空間劃分為多個叢集，查詢時僅搜尋最相關的叢集；HNSW（Hierarchical Navigable Small World）基於圖結構的分層導航，在高召回率與低延遲間取得優秀平衡；Annoy（Approximate Nearest Neighbors Oh Yeah）使用隨機投影樹，適合靜態資料集且記憶體效率較高。選擇索引類型需依據資料規模、更新頻率與延遲需求綜合考量。

## 向量資料庫 vs. 傳統資料庫加向量擴展

專用向量資料庫（如 Pinecone、Weaviate、Qdrant、Milvus）針對向量檢索場景最佳化，提供即時索引更新、後設資料篩選、以及分散式擴展能力。另一條路徑是在傳統資料庫上加掛向量擴展（如 PostgreSQL 的 pgvector），優勢在於減少系統複雜度，讓向量資料與結構化資料共存於同一資料庫中。對於中小規模的應用，後者往往是更務實的選擇；而在需要處理數十億級向量且要求毫秒級延遲的場景，專用向量資料庫則更具優勢。

## 相似度指標

向量間的相似度計算有三種主要指標。Cosine similarity 衡量向量方向的一致性，不受向量長度影響，是最廣泛使用的指標。Dot product（內積）同時考慮方向與大小，適合向量已正規化或需要考慮重要性權重的場景。L2 distance（歐幾里得距離）衡量向量空間中的直線距離，對向量大小的差異敏感。多數嵌入模型產出的向量已經正規化，此時 cosine similarity 與 dot product 等價。

## 效能權衡

索引建置（indexing）與查詢（querying）之間存在效能權衡。更精細的索引能提供更高的查詢精度與速度，但需要更長的建置時間與更多的記憶體。在 [[rag]] 應用中，需根據知識庫的更新頻率與查詢量來調整這一平衡。

## Related

- [[rag]]
- [[ai-agent]]
- [[memory-management]]
