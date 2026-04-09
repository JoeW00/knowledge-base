---
title: AI 記憶系統 (AI Memory System)
tags: [agent, memory, vector-database, local-first, architecture]
created: 2026-04-09
updated: 2026-04-09
---

# AI 記憶系統 (AI Memory System)

AI 記憶系統是一種讓 AI 助手在多次對話間保留、組織與檢索先前互動資訊的基礎設施。核心問題是：使用者與 AI 累積的大量對話在 session 結束後即消失，但其中包含了重要的決策脈絡、除錯經驗與偏好設定。

## 設計挑戰

六個月的日常 AI 使用約累積 1950 萬 tokens，遠超任何模型的 context window。直接全部載入不可能，LLM 摘要方式年成本約 $500。有效的記憶系統需要解決三個問題：如何以極少的 token（理想上 <200）載入足夠的上下文啟動對話；如何在需要時精準檢索相關記憶；以及如何在不依賴雲端 API 的情況下完成上述功能。

## MemPalace 的方法

MemPalace 以「記憶宮殿」隱喻建構層級結構：Wing（人物/專案命名空間）→ Room（具體主題）→ Hall（記憶類型通道）→ Closet/Drawer（細粒度存儲）。這種結構化設計讓 [[vector-search]] 在檢索時可先過濾 Wing 和 Room，將搜尋範圍縮小到相關子集，帶來 +34% 的召回率提升。

其「原文逐字儲存」策略挑戰了多數記憶系統依賴 LLM 摘要的做法——直接保存原始對話反而在 LongMemEval 基準測試中達到 96.6% 召回率，且完全不需要 API 呼叫。多層壓縮機制（AAAK）讓系統能以 ~170 tokens 的 wake-up context 啟動對話，按需再透過搜尋載入更多記憶。

## 與 Agent 架構的關係

記憶系統是 [[ai-agent]] 架構中 [[memory-management]] 的具體實現。在 agentic 工作流中，記憶系統讓 agent 能跨 session 累積知識、追蹤長期專案狀態、記住使用者偏好，從「無狀態的問答工具」演化為「有持續性的協作夥伴」。MemPalace 透過 Claude Code hooks 實現自動記錄，是記憶系統與開發工作流深度整合的實例。

## 在學術分類體系中的定位

根據 [[agent-memory-taxonomy]]（Forms–Functions–Dynamics 框架），MemPalace 的層級結構屬於 [[token-level-memory]] 中的 hierarchical (3D) 組織形式，其功能定位橫跨 factual memory（保存使用者偏好與事實）和 [[experiential-memory]]（累積除錯經驗）。其「原文逐字儲存 + 按需搜尋」的設計則體現了 [[memory-lifecycle]] 中 formation（結構化建構）與 retrieval（語義檢索）的結合。

## Related

- [[memory-management]]
- [[vector-search]]
- [[ai-agent]]
- [[embedding]]
- [[agent-memory-taxonomy]]
- [[token-level-memory]]
- [[memory-lifecycle]]
- [[mempalace-report-summary]]
- [[agent-memory-survey-summary]]
