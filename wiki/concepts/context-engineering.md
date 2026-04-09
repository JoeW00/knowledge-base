---
title: Context Engineering（上下文工程）
tags: [agent-memory, context-window, architecture, mcp]
created: 2026-04-09
updated: 2026-04-09
---

# Context Engineering

Context engineering 是一種系統性設計方法論，將 LLM 的上下文窗口視為**受限的計算資源**，嚴格最佳化注入其中的資訊負載——包括指令、知識、狀態與記憶——以彌補模型巨大的輸入容量與有限的生成能力之間的不對稱。

## 與 Agent Memory 的關係

Context engineering 和 agent memory 的交集主要出現在**工作記憶管理**——兩者使用幾乎相同的技術手段處理有限上下文窗口的約束：token pruning、importance-based selection、rolling summary、動態資訊檢索與遞迴狀態更新。在長期互動的 working memory 場景中，「工程化上下文」與「維護 agent 短期記憶」的邊界實際上已經消融。

然而兩者在範疇上有本質區別。Context engineering 運作在**資源管理範式**下——關注如何正確格式化指令、排程工具呼叫、確保中間狀態在上下文窗口內有效呈現，核心目標是語法正確性與執行效率。Agent memory 則運作在**認知建模範式**下——涵蓋事實知識的持久儲存、經驗軌跡的累積與演化、甚至將記憶內化到模型參數中，核心目標是 agent 的持續學習與身份連貫。

簡言之：context engineering 建構讓 agent 在資源約束下感知與行動的**外部鷹架**，agent memory 則維持支撐學習、適應與自主性的**內部認知基底**。前者最佳化單次上下文窗口的資訊利用率，後者維護跨越任意多個上下文窗口的持續認知狀態。

## 實務上的技術交疊

工具整合推理（tool-integrated reasoning）與標準化通訊協定（如 MCP）被歸類為 context engineering 的範疇——它們關注的是如何將工具呼叫結果正確注入上下文。而 [[memory-lifecycle]] 中的 formation、evolution、retrieval 則屬於 agent memory 的範疇。兩者共享 [[vector-search]]、語義壓縮等基礎技術，但目的與時間尺度不同。

## Related

- [[agent-memory-taxonomy]]
- [[memory-management]]
- [[prompt-engineering]]
- [[rag]]
- [[kv-cache]]
- [[agent-memory-survey-summary]]
