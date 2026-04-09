---
title: Attention Mechanism
tags: [architecture, transformer, deep-learning]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

Attention mechanism（注意力機制）是 [[transformer-architecture]] 的核心組件，允許模型在處理序列時動態權衡不同輸入 token 的重要性。相較於傳統 seq2seq 模型將整個輸入壓縮為固定長度向量的做法，注意力機制讓模型能夠直接存取輸入序列的任意位置，徹底解決了長序列資訊瓶頸問題。

## Query-Key-Value 運算

注意力機制的數學核心是 Query-Key-Value（QKV）運算。每個 token 的表徵被線性投影為三個向量：Query（查詢）、Key（鍵）與 Value（值）。注意力權重透過 Query 與所有 Key 的點積計算得出，經 softmax 歸一化後，用於對 Value 進行加權求和。直覺上，Query 代表「我在找什麼」，Key 代表「我包含什麼」，而 Value 則是實際傳遞的資訊內容。公式為 Attention(Q,K,V) = softmax(QK^T / √d_k)V。

## Multi-Head Attention

Multi-head attention 將 QKV 運算拆分為多個平行的「注意力頭」（attention head），每個頭獨立學習不同的注意力模式。例如，某些頭可能專注於語法結構，另一些則捕捉語義關聯。多個頭的輸出串聯後再透過線性層整合。這種設計讓模型能夠同時從多個面向理解 token 之間的關係，大幅提升表達能力。

## Cross-Attention vs. Self-Attention

Self-attention 中，Query、Key、Value 均來自同一序列，用於序列內部的 token 間互動。Cross-attention 則讓一個序列的 Query 與另一個序列的 Key 和 Value 互動，常見於 encoder-decoder 架構中——decoder 透過 cross-attention 存取 encoder 的輸出。在多模態模型中，cross-attention 也用於讓文字 token 關注影像特徵。

## 計算複雜度與最佳化方向

標準 self-attention 的計算複雜度為 O(n²)，其中 n 為序列長度。這意味著序列長度每增加一倍，計算量增加四倍，成為處理長文件的主要瓶頸。為此，研究社群提出了多種最佳化方案：Flash Attention 透過最佳化記憶體存取模式在不改變結果的情況下加速運算；Multi-Query Attention（MQA）與 Grouped-Query Attention（GQA）透過共享 Key/Value 頭減少記憶體使用與計算量；稀疏注意力則限制每個 token 僅關注部分位置以降低複雜度。

## Related

- [[transformer-architecture]]
- [[foundation-model]]
- [[scaling-law]]
- [[sampling-strategy]]
- [[ch02-understanding-foundation-models-summary]]
