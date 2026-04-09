---
title: Sampling Strategy
tags: [inference, generation, foundation-model]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

Sampling strategy（取樣策略）是指模型在生成文字時，從機率分佈中選取下一個 token 的方法。不同的取樣策略會顯著影響生成結果的品質、多樣性與一致性。理解取樣機制對於建構可靠的 AI 應用至關重要，因為它直接決定了模型輸出的行為特性。

## Greedy Decoding vs. Random Sampling

最簡單的策略是 greedy decoding（貪婪解碼），每一步都選擇機率最高的 token。這種方法確定性強、結果可重現，但容易產生重複且缺乏創造力的文字。Random sampling（隨機取樣）則根據完整的機率分佈隨機抽取 token，引入了多樣性但也可能產生不連貫或離題的內容。實務上，工程師需要在這兩個極端之間找到適當的平衡點。

## Temperature 的效果

Temperature（溫度）是控制機率分佈銳利程度的超參數。溫度值為 1 時保持原始分佈；低於 1 時分佈變得更尖銳，高機率 token 被進一步強化，生成結果更確定但也更保守；高於 1 時分佈變得更平坦，低機率 token 獲得更多機會，生成結果更多樣但也更不可預測。對於事實性問答等任務，通常使用較低溫度；對於創意寫作等任務，則傾向使用較高溫度。

## Top-k 與 Top-p（Nucleus Sampling）

Top-k sampling 限制模型只從機率最高的 k 個 token 中取樣，排除長尾分佈中的低機率選項。然而固定的 k 值無法適應不同情境——有些位置的合理選項很少，有些則很多。Top-p sampling（又稱 nucleus sampling）透過動態調整候選集大小解決了這個問題：它選取累積機率達到 p 值的最小 token 集合。例如 top-p = 0.9 意味著從涵蓋 90% 機率質量的 token 子集中取樣，候選集大小隨上下文自動調整。

## Test-Time Compute（推理時計算）

Test-time compute 是近年來備受關注的新方向，透過在推理階段投入更多計算資源來提升輸出品質。典型做法包括讓模型生成多個候選回應再選擇最佳者、使用 chain-of-thought 延長推理過程、或以 tree search 方式探索多條推理路徑。這種方法將部分能力提升的責任從預訓練階段轉移到推理階段，提供了 [[scaling-law]] 之外的另一條效能提升途徑。

## 結構化輸出

在應用開發中，開發者經常需要模型產生符合特定格式的輸出（如 JSON、XML 等）。結構化輸出技術透過在 sampling 過程中施加語法約束，確保生成的 token 序列符合預定義的格式規範。這對於將 AI 模型整合進既有系統至關重要，因為下游程式需要能可靠地解析模型輸出。

## Sampling 作為創造力與不一致性的根源

Sampling 的機率本質是一把雙刃劍。一方面，它賦予模型「創造力」——相同的 prompt 可以產生不同的回應，使模型能夠探索多種可能的表達方式。另一方面，這種不確定性也是不一致性的根源——同一個問題在不同時間可能得到不同甚至矛盾的回答，這對需要可靠性的應用場景構成挑戰，也與 [[hallucination]] 問題密切相關。

## Related

- [[transformer-architecture]]
- [[foundation-model]]
- [[hallucination]]
- [[attention-mechanism]]
- [[ch02-understanding-foundation-models-summary]]
