---
title: Agent Memory 分類體系 (Forms–Functions–Dynamics)
tags: [agent-memory, taxonomy, survey, architecture]
created: 2026-04-09
updated: 2026-04-09
---

# Agent Memory 分類體系

Agent memory 分類體系是由 Hu et al. (2025) 在 "Memory in the Age of AI Agents" 綜述中提出的統一框架，以 **Forms（形式）–Functions（功能）–Dynamics（動態）** 三個正交維度組織當前 agent 記憶研究。此框架旨在取代傳統「長期 vs. 短期記憶」的粗略二分法，提供更精確的概念地圖。

## 三個維度

**Forms（記憶的載體）** 回答「什麼承載記憶？」：[[token-level-memory]]（顯式離散單元）、parametric memory（模型參數中的統計模式）、latent memory（隱藏狀態中的連續表示）。三者在透明度、可編輯性與整合深度上各有取捨——token-level 最易理解與修改，parametric 最緊密地嵌入推理流程，latent 則介於兩者之間且適合跨模態融合。

**Functions（記憶的目的）** 回答「為何需要記憶？」：factual memory 儲存使用者偏好與環境狀態以確保一致性；[[experiential-memory]] 從成功與失敗的軌跡中提煉策略與技能以支持自我進化；working memory 在單一任務中管理有限的上下文空間以支持即時推理。三者構成 encoding–processing–retrieval 的認知循環。

**Dynamics（記憶的運作）** 回答「記憶如何運轉與演化？」：[[memory-lifecycle]] 由 formation（提取）、evolution（整合與遺忘）、retrieval（查詢與取回）三個過程組成，彼此形成閉環驅動 agent 的持續學習。

## 與相鄰概念的邊界

此框架明確區分了 agent memory 與三個常被混淆的概念：LLM memory（模型內部的 KV cache 與長上下文處理）、[[rag]]（靜態知識的增強檢索）、[[context-engineering]]（上下文窗口的資源最佳化）。Agent memory 的獨特性在於它是 **持續演化的認知狀態**，而非單次推理的資訊注入。

## 前沿方向

論文提出七個研究前沿：從 retrieval 到 generative memory 的典範轉移、自動化記憶管理、RL 驅動的記憶系統、多模態記憶、多 agent 共享記憶、世界模型中的記憶、以及可信賴記憶（隱私、可解釋性、幻覺穩健性）。

## Related

- [[token-level-memory]]
- [[experiential-memory]]
- [[memory-lifecycle]]
- [[context-engineering]]
- [[ai-memory-system]]
- [[memory-management]]
- [[rag]]
- [[agent-memory-survey-summary]]
