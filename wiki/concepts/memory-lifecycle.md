---
title: Memory Lifecycle（記憶生命週期）
tags: [agent-memory, dynamics, formation, evolution, retrieval]
created: 2026-04-09
updated: 2026-04-09
---

# Memory Lifecycle

Memory lifecycle 是 agent 記憶系統的動態維度，描述記憶從原始經驗到可用知識的完整運作循環。不同於靜態編碼在模型參數中或固定資料庫裡的知識，agentic memory 能**自主建構、更新記憶庫並依查詢客製化檢索**，這種自適應能力是 agent 實現自我演化與終身學習的基礎。

## 三個基本過程

**Memory Formation（記憶形成）**：將原始互動經驗轉化為資訊密度高的知識單元。五種形成操作依壓縮粒度排列：semantic summarization（語義摘要，保留全域語義）、knowledge distillation（知識蒸餾，提取特定事實或策略）、structured construction（結構化建構，組織為圖或樹拓撲）、latent representation（潛在表示，編碼為連續向量）、parametric internalization（參數內化，透過 fine-tuning 寫入權重）。

**Memory Evolution（記憶演化）**：將新形成的記憶整合進既有記憶庫。三個核心機制——consolidation（合併冗餘項）、updating（解決衝突並更新過時資訊）、forgetting（主動修剪低效用記憶以維持檢索效率）。此過程確保記憶庫在不斷變化的環境中保持一致性與可泛化性。

**Memory Retrieval（記憶檢索）**：依據當前上下文建構任務感知查詢並取回相關記憶。涉及四個設計決策：retrieval timing（何時檢索——任務初始化、間歇性、或持續性）、query construction（如何建構查詢）、retrieval strategies（檢索策略——語義搜尋、圖遍歷、混合）、post-retrieval processing（後處理——重排、摘要、過濾）。

## 循環互動

三個過程並非線性流程，而是形成閉環：推理結果與環境反饋回流到 formation 提取新 insight，再經 evolution 更新記憶庫，持續驅動 agent 的認知演化。短期與長期記憶的區別不來自架構分離，而來自 formation / evolution / retrieval 被觸發的**時間模式**——同一個記憶容器可同時支持任務內（short-term）和跨任務（long-term）的記憶行為。

## RL 對 lifecycle 的影響

記憶管理正從手工規則（heuristic）→ RL 輔助（部分操作由 RL 訓練）→ 全 RL 驅動演進。Mem1 和 MemAgent 用 PPO/GRPO 訓練摘要能力，Mem-α 以 RL 訓練 insight 提取，MemSearcher 和 Context Folding 以 RL 最佳化工作記憶管理。最終願景是 agent 自主設計記憶架構，最小化人為先驗。

## Related

- [[agent-memory-taxonomy]]
- [[token-level-memory]]
- [[experiential-memory]]
- [[memory-management]]
- [[reinforcement-learning]]
- [[agent-memory-survey-summary]]
