---
title: "評估流水線（Evaluation Pipeline）"
tags: [evaluation, methodology, ai-engineering]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

評估流水線是一套系統化、可重複執行的 AI 評估流程，將評估從臨時性的手動檢查提升為工程化的持續實踐。它是 [[evaluation-driven-development]] 理念落地的核心基礎設施，確保團隊能在每次模型更新或系統修改後快速獲得可靠的品質回饋。

## 元件級評估 vs. 端到端評估

一個完整的 AI 系統通常由多個元件組成（如檢索模組、生成模組、後處理模組），評估可以在不同層級進行。**元件級評估**單獨測試每個模組的表現，例如檢索模組的召回率、生成模組的 [[factual-consistency]]。**端到端評估**則測試整個系統從輸入到輸出的整體品質。兩者互補——元件級評估有助於快速定位問題根源，端到端評估確保各模組協同工作時的整體表現。

## 評估資料集的建構

高品質的評估資料集是流水線的基礎。建構時需要注意：資料應覆蓋實際應用中的各種場景（包括邊界情況和困難案例）、標注品質需要透過多人標注與一致性檢查來保障、資料集規模應足以產生統計顯著的結果。此外，評估資料集應定期更新以避免過擬合，並保持私密以防止 [[benchmark-contamination]]。

## 自動化評估與人工審核

成熟的評估流水線會結合自動化評估與人工審核。自動化評估（包括 [[functional-correctness]] 驗證、[[lexical-semantic-similarity]] 計算、[[llm-as-judge]] 評分）可以在每次程式碼變更後即時執行，提供快速回饋。人工審核則定期進行，對自動化評估可能遺漏的品質面向（如文化敏感度、語氣適切性）進行補充評估。

## 自助抽樣驗證可靠性

評估結果本身的可靠性也需要被驗證。自助抽樣（bootstrap sampling）是常用的統計方法：從評估結果中反覆隨機抽取樣本，計算每次抽樣的指標值，藉此估算指標的信賴區間。如果信賴區間過寬，表示目前的評估資料量不足以得出可靠結論，需要擴充資料集。

## AI 指標與業務指標的關聯

評估流水線的最終目標是連結技術指標與業務價值。AI 層面的指標（如準確率、BLEU 分數）需要與業務指標（如使用者滿意度、轉換率、客服工單減少量）建立明確的對應關係。只有當 AI 指標的提升能夠反映在業務成果上，評估流水線才真正發揮其價值。

## Related

- [[evaluation-driven-development]]
- [[llm-as-judge]]
- [[factual-consistency]]
- [[functional-correctness]]
- [[lexical-semantic-similarity]]
- [[model-selection-workflow]]
- [[benchmark-contamination]]
