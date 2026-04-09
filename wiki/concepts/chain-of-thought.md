---
title: Chain-of-Thought
tags: [prompt-engineering, reasoning, methodology]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Chain-of-thought（CoT）是一種要求模型在給出最終答案之前，先逐步展開推理過程的提示技術。由 Wei et al.（2022）首次系統性提出，研究發現在提示詞中加入推理步驟的範例，能大幅提升模型在數學推理、邏輯判斷、常識推理等需要多步思考的任務上的表現。CoT 的核心洞見在於：讓模型「想出來」比讓模型「猜出來」更可靠。

## Zero-shot CoT

除了透過 few-shot 範例示範推理過程之外，Kojima et al. 發現僅在提示詞末尾加上「Let's think step by step」這句簡單指令，就能有效觸發模型的逐步推理行為，這就是所謂的 zero-shot CoT。這個發現意義重大，因為它表明大型語言模型已經具備逐步推理的潛力，只需要適當的提示就能激發。Zero-shot CoT 在實作上極為便利，是最常用的 CoT 變體之一。

## Self-consistency 與延伸

Self-consistency 是 CoT 的重要強化策略：對同一問題進行多次採樣（調高 temperature），然後從多條推理路徑中取多數決作為最終答案。這種方法能有效降低單次推理可能出現的錯誤。在此基礎上，學術界進一步發展出 tree-of-thought（ToT）與 graph-of-thought（GoT）等延伸框架，允許模型在推理過程中分支探索、回溯修正，模擬更接近人類的複雜思維過程。

## 對幻覺的抑制效果

CoT 不僅能提升推理正確率，還能有效減少模型的幻覺現象。當模型被要求逐步推理時，每一步都需要基於前一步的結論，這種連貫性約束使得模型更難「跳過」邏輯環節直接編造答案。此外，展開的推理過程也讓開發者更容易檢查模型的思路是否合理，提升了系統的可解釋性與可除錯性。

## Test-time Compute 的關聯

CoT 與 test-time compute（推理時運算）的概念密切相關。傳統上，提升模型能力主要依靠增加訓練時的運算量（更大的模型、更多的資料）。而 CoT 開啟了另一條路徑：透過在推理階段投入更多運算（更長的推理步驟、多次採樣），在不改變模型參數的前提下提升輸出品質。這一思路對 AI 系統設計有深遠的架構影響。

## Related

- [[prompt-engineering]]
- [[in-context-learning]]
- [[react-framework]]
- [[ai-agent]]
