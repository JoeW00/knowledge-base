---
title: Token-level Memory（token 層級記憶）
tags: [agent-memory, architecture, knowledge-graph, retrieval]
created: 2026-04-09
updated: 2026-04-09
---

# Token-level Memory

Token-level memory 是 agent 記憶三大形式中最常見且研究最多的一類，將資訊儲存為**持久、離散、可外部存取**的單元。此處「token」是廣義概念——不限於文字 token，亦涵蓋視覺 token、音訊幀等任何可寫入、檢索、重組、修改的離散元素。其核心特性是透明性、可編輯性與可解釋性，使其成為檢索、路由、衝突處理的自然介面層。

## 三種拓撲結構

依據記憶單元之間的組織方式，token-level memory 從無拓撲到多層拓撲分為三級：

**Flat Memory (1D)**：記憶以序列或集合形式累積，單元之間不建立顯式關係。常見用途包括對話日誌、經驗池（如 ExpeL、Reflexion）、使用者檔案。優點是實現簡單，缺點是隨記憶量增長檢索效率下降。

**Planar Memory (2D)**：在單一層面上建立結構——圖、樹、表格等。例如 A-MEM 將知識組織為相互連結的筆記網路，KGT 以知識圖譜追蹤使用者偏好，MemTree 以樹狀結構支持層級檢索。2D 結構讓 agent 能進行關係推理，但維護成本較高。

**Hierarchical Memory (3D)**：跨多個層級建立連結，形成分層或體積化記憶。GraphRAG 透過社群偵測建立多層圖索引，HippoRAG 以海馬迴為靈感結合語義與情節圖層，HiAgent 設計工作記憶 / 短期 / 長期三層架構。3D 結構支持多粒度檢索與跨層推理，是處理複雜長期任務的最先進形式。

## 與其他記憶形式的比較

相較於 parametric memory（編碼在模型權重中，隱式存取）和 latent memory（連續隱藏狀態，推理期間動態更新），token-level memory 最大的優勢是**人類可讀且可直接修改**。這使得除錯、審計與信任建立更為容易，但代價是需要額外的檢索與上下文組裝步驟。

## Related

- [[agent-memory-taxonomy]]
- [[memory-lifecycle]]
- [[rag]]
- [[vector-search]]
- [[memory-management]]
- [[agent-memory-survey-summary]]
