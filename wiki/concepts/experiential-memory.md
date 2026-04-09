---
title: Experiential Memory（經驗記憶）
tags: [agent-memory, self-evolution, reinforcement-learning, lifelong-learning]
created: 2026-04-09
updated: 2026-04-09
---

# Experiential Memory

Experiential memory 是 agent 記憶功能分類中的第二大支柱，回答的核心問題是「agent 如何從過往經驗中自我提升？」。不同於 factual memory 儲存靜態事實，experiential memory 從任務執行的成功與失敗軌跡中提煉出**可重用的程序性與策略性知識**，是 agent 實現持續學習（continual learning）和跨任務遷移的關鍵機制。

## 三種子類型

**Case-based Memory（案例記憶）**：直接儲存完整或部分的任務軌跡作為參考案例。ExpeL 同時保留成功與失敗案例並從中對比提取 insight；Reflexion 將過往嘗試的反思作為長期記憶供下次任務參考。優點是保留豐富的上下文細節，缺點是案例庫膨脹後檢索成本高。

**Strategy-based Memory（策略記憶）**：從軌跡中抽象出高階規劃策略與反思規則。AWM 從成功軌跡中提取通用工作流（workflow）；H2R 設計了雙層反思機制——既提煉整體規劃 insight 又提取步驟級執行 insight。ReasoningBank 累積可遷移的推理策略。這類記憶犧牲具體性換取泛化能力。

**Skill-based Memory（技能記憶）**：將經驗固化為可執行的程式碼或函式。Voyager 在 Minecraft 中將學到的能力存為可呼叫的 JavaScript 技能庫；DGM 以遞迴自修改的程式碼庫實現技能的持續演化。技能記憶是最「可操作」的經驗形式，但需要 agent 具備程式生成能力。

## 提煉方法的演進

早期方法依賴固定 prompt 引導 LLM 反思（prompt-driven），效果受限於 prompt 設計品質。近期趨勢轉向**可訓練的提煉模組**：Memory-R1 以專用 LLMExtract 模組提取經驗知識，Mem-α 則透過 RL 明確訓練 LLM 學會「提取什麼」與「如何保存」，將 insight 提取能力內化為模型的固有能力。

## 與 factual / working memory 的互動

三種功能記憶並非孤立運作。Working memory 在任務執行中蒐集中間結果，任務結束後經驗記憶從中提煉策略；下次任務啟動時，working memory 又從經驗記憶中檢索相關策略載入上下文。這構成了 encoding–processing–retrieval 的認知循環。

## Related

- [[agent-memory-taxonomy]]
- [[memory-lifecycle]]
- [[memory-management]]
- [[reinforcement-learning]]
- [[ai-agent]]
- [[agent-memory-survey-summary]]
