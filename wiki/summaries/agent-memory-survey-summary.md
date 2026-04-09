---
title: "Memory in the Age of AI Agents: A Survey — 摘要"
source: raw/papers/arxiv_2512_13564.pdf
tags: [agent-memory, survey, taxonomy, reinforcement-learning, multi-agent]
created: 2026-04-09
updated: 2026-04-09
---

# Memory in the Age of AI Agents: A Survey — 摘要

本論文由新加坡國立大學、人民大學、復旦大學等多校團隊合作，提出了一套以 **Forms–Functions–Dynamics** 三維度統一框架分析 AI agent 記憶系統的完整分類體系。論文首先釐清 agent memory 與 LLM memory、[[rag]]、[[context-engineering]] 三個相關概念的邊界，再從「記憶的載體」（forms）、「記憶的目的」（functions）、「記憶的運作」（dynamics）三個面向展開系統性回顧，涵蓋超過 300 篇相關文獻。

## 關鍵論點

1. **三種記憶形式**：[[token-level-memory]]（顯式離散單元，又分 1D flat / 2D planar / 3D hierarchical）、parametric memory（編碼在模型參數中）、latent memory（隱藏狀態中的連續表示）構成 agent 記憶的結構基礎。

2. **三種功能分類**：超越傳統「長短期記憶」的粗略劃分，提出 factual memory（事實知識：使用者偏好與環境狀態）、[[experiential-memory]]（經驗知識：策略、技能、案例）、working memory（工作記憶：任務中的暫態上下文管理）的細粒度分類。

3. **記憶生命週期**：[[memory-lifecycle]] 由三個基本過程組成——formation（從原始經驗提取知識）、evolution（整合、去衝突、遺忘）、retrieval（依上下文建構查詢並取回相關記憶），三者形成持續循環驅動 agent 自我演化。

4. **從檢索到生成的典範轉移**：未來記憶系統將從 retrieval-centric 轉向 generative memory，agent 按需合成記憶表示而非僅檢索靜態儲存，實現上下文自適應與跨模態融合。

5. **RL 驅動的記憶管理**：記憶系統正從手工規則（RL-free）→ 部分 RL 輔助 → 全 RL 驅動演進，未來 agent 將自主設計記憶架構與管理策略，最小化人為先驗假設。

## 原始論文

- 路徑：`raw/papers/arxiv_2512_13564.pdf`
- GitHub：https://github.com/Shichun-Liu/Agent-Memory-Paper-List
- arXiv：2512.13564

## Related

- [[agent-memory-taxonomy]]
- [[token-level-memory]]
- [[experiential-memory]]
- [[memory-lifecycle]]
- [[context-engineering]]
- [[ai-memory-system]]
- [[memory-management]]
- [[rag]]
