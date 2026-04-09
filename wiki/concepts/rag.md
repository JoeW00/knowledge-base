---
title: RAG (Retrieval-Augmented Generation)
tags: [rag, retrieval, ai-engineering, architecture]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Retrieval-Augmented Generation（RAG）是一種透過從外部記憶源檢索相關資訊來增強語言模型生成品質的架構模式。其基本運作流程由兩個核心元件組成：檢索器（retriever）負責從知識庫中找出與查詢最相關的文件片段，生成器（generator）則將這些檢索結果作為上下文，結合原始問題產出更精確、更有根據的回應。

## 為何 RAG 有效

RAG 解決了大型語言模型的幾個根本限制。首先，它能有效降低幻覺，因為模型可以基於檢索到的真實資料回答問題，而非僅依賴訓練時記憶的知識。其次，RAG 讓模型能存取最新資訊，突破訓練資料的時間截止限制。第三，RAG 減少了模型需要記憶的知識量，使得較小的模型也能處理專業領域問題。即使上下文窗口持續擴大，RAG 仍具有降低成本與提升效率的獨特價值——將百萬 token 全部塞入上下文既昂貴又低效。

## 檢索演算法

主流的檢索方法分為三大類。BM25 等詞項檢索（term-based retrieval）基於詞頻統計，對精確關鍵字匹配效果優異且無需訓練。[[vector-search]] 基於嵌入向量的語義檢索能捕捉語義相似性，即使查詢與文件用詞不同也能匹配。混合搜尋（hybrid search）結合兩者的優勢，先分別召回結果再透過融合策略（如 Reciprocal Rank Fusion）合併排序，在實務中通常表現最佳。

## 分塊策略與最佳化

文件分塊（chunking）是 RAG 管線中的關鍵步驟。分塊過小會失去語境，過大則引入噪音且增加成本。常見策略包括固定大小分塊（附帶重疊）、基於段落或章節的語義分塊、以及遞迴分塊。重排序（reranking）在初步檢索後使用更精確的模型對候選結果重新排序，能顯著提升最終精度。查詢重寫（query rewriting）透過改寫使用者查詢來改善檢索效果。上下文檢索（contextual retrieval）則在每個分塊中加入文件級上下文資訊，提升獨立分塊的可理解性。

## RAG 與 Agent Memory 的區別

根據 [[agent-memory-taxonomy]]，RAG 與 agent memory 在技術上大量重疊（都使用向量索引、語義搜尋、上下文擴展），但在應用範疇上有本質差異。傳統 RAG 主要為單次推理提供靜態外部知識的增強檢索；agent memory 則在 agent 的持續互動中不斷累積、演化自身的記憶庫。隨著 agentic RAG（如 PlanRAG、Self-RAG）的出現，兩者的邊界日趨模糊——agent 自主控制何時、如何檢索，且檢索結果會回流更新記憶庫。Graph RAG（如 LightRAG、HippoRAG）同時被 RAG 和 memory 社群引用，體現了兩個領域的融合趨勢。

## Related

- [[vector-search]]
- [[ai-agent]]
- [[prompt-engineering]]
- [[memory-management]]
- [[agent-memory-taxonomy]]
- [[context-engineering]]
- [[agent-memory-survey-summary]]
