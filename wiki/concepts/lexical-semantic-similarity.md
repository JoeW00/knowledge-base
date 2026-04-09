---
title: "詞彙與語義相似度（Lexical & Semantic Similarity）"
tags: [evaluation, nlp, metrics]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

詞彙與語義相似度是衡量兩段文本之間相近程度的兩大類方法。在 AI 評估中，經常需要比較模型生成的文本與參考答案之間的相似程度，這兩類方法各有其適用場景與局限性。

## 詞彙相似度指標

詞彙相似度（lexical similarity）透過分析文字表面的重疊程度來衡量相似性。常見的指標包括：

- **BLEU**：最初為機器翻譯設計，計算生成文本中的 n-gram 在參考文本中出現的比例。BLEU 側重精確率（precision），適合評估翻譯品質，但對同義詞和語序變化不敏感。
- **ROUGE**：為文本摘要設計，計算參考文本中的 n-gram 在生成文本中被涵蓋的比例。ROUGE 側重召回率（recall），常見變體包括 ROUGE-1、ROUGE-2 和 ROUGE-L。
- **METEOR**：改進了 BLEU 的不足，加入了同義詞匹配、詞幹還原等機制，與人類判斷的相關性較高。

詞彙相似度指標的主要局限在於它們無法捕捉語義等價性。例如「這部電影很棒」與「這部影片相當出色」在詞彙層面重疊很少，但語義幾乎完全相同。

## 語義相似度指標

語義相似度（semantic similarity）透過 [[embedding]] 向量空間中的距離來衡量文本含義的接近程度。最常見的方法是計算兩段文本 embedding 的餘弦相似度（cosine similarity）。此外，**BERTScore** 是一種結合了 contextual embedding 的指標，它計算生成文本與參考文本在 token 層級的最佳匹配，能更細緻地捕捉語義對應關係。

## 適用與不適用場景

詞彙相似度適合用於翻譯、摘要等有明確參考答案的任務。語義相似度則更適合開放式生成、問答等答案形式多樣的場景。兩類方法都不太適合需要深度推理判斷的評估，此時應考慮 [[llm-as-judge]] 或人類評估。

## Related

- [[perplexity]]
- [[embedding]]
- [[llm-as-judge]]
- [[functional-correctness]]
- [[evaluation-pipeline]]
