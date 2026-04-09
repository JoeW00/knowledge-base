---
title: "第06章：RAG 與智慧體 摘要"
source: "raw/articles/ai_engineering_chapters/03-提示工程與RAG/第06章-RAG與智慧體.md"
tags: [ai-engineering, rag, agent, retrieval]
created: 2026-04-09
updated: 2026-04-09
---

## 摘要

本章介紹兩種為模型構建上下文的核心模式：RAG 與 agent。[[rag]] 透過檢索器從外部記憶源擷取相關資訊，再由生成器產出回應；詳述基於詞項（BM25）與基於嵌入的檢索演算法、分塊策略、重排序、查詢重寫等最佳化技巧。Agent 部分定義智慧體為能感知並作用於環境的實體，探討工具使用、[[react-framework]] 與 Reflexion 等框架、故障模式與評估。最後介紹記憶系統（短期/長期記憶）對兩者的支撐作用。

## 重點整理

1. [[rag]] 不會因上下文長度增長而被淘汰，仍有助降低成本與提升效率
2. 混合搜尋結合詞項與嵌入檢索的優勢
3. [[ai-agent]] 的成功取決於工具庫與規劃能力，錯誤具累積效應
4. 規劃應與執行解耦以避免無效路徑
5. [[memory-management]] 是智慧體運作的關鍵基礎設施

## Related

- [[rag]]
- [[vector-search]]
- [[ai-agent]]
- [[function-calling]]
- [[react-framework]]
- [[memory-management]]
