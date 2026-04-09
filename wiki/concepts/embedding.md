---
title: "Embedding（嵌入向量）"
tags: [nlp, representation, vector-search]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

Embedding 是將文字、圖片等非結構化資料轉換為固定維度數值向量的技術，使得語義相近的資料在向量空間中彼此靠近。這項技術是現代自然語言處理與資訊檢索的基石，廣泛應用於語義搜尋、分類、聚類等任務。

## 從 Word2Vec 到 Contextual Embedding

Embedding 技術經歷了重要的演進歷程。早期的 **Word2Vec**（2013）和 **GloVe** 為每個詞彙分配一個固定的向量，無法處理一詞多義的問題——例如「bank」在「河岸」與「銀行」兩個語境中會得到相同的向量。**Contextual embedding**（如 BERT、GPT 系列）的出現解決了這個問題，同一個詞在不同上下文中會得到不同的向量表示，能更精確地捕捉語義。現代的 embedding model（如 OpenAI text-embedding-3、Cohere embed）通常基於 Transformer 架構，能夠將整段文字編碼為一個語義豐富的向量。

## 主要用途

Embedding 的應用範圍非常廣泛。**語義相似度計算**：透過餘弦相似度（cosine similarity）比較兩個向量的方向，判斷兩段文字的語義接近程度，這也是 [[lexical-semantic-similarity]] 中語義層面的核心技術。**向量搜尋（vector search）**：將文件庫中的所有文本轉為向量後，可以快速找到與查詢語義最相關的文件，是 RAG 系統的關鍵元件。**分類與聚類**：將文本轉為向量後，可以用傳統機器學習方法進行分類或發現潛在主題群組。

## 模型選擇與維度權衡

選擇 embedding model 時需要考慮通用模型與領域特定模型的取捨。通用模型適用範圍廣，但在特定領域（如醫療、法律）的表現可能不如經過領域微調的模型。此外，向量維度也是重要考量——較高維度通常能捕捉更豐富的語義資訊，但會增加儲存成本與搜尋延遲。實務上需要根據應用場景在語義品質與效能之間取得平衡。

## Related

- [[lexical-semantic-similarity]]
- [[perplexity]]
- [[evaluation-pipeline]]
