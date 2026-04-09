---
title: Self-Supervised Learning
tags: [training, foundation-model, machine-learning]
created: 2026-04-09
updated: 2026-04-09
---

## 定義與原理

Self-supervised learning（自監督學習）是一種訓練方式，模型從輸入資料自身的結構中推導出監督信號，無需人工標註。其核心思想是利用資料中固有的規律作為學習目標——例如遮蔽部分文字讓模型預測被遮蔽的內容，或是根據前文預測下一個 token。這種方法讓模型能夠從資料本身學習語義表徵，而不依賴昂貴且難以規模化的人工標註。

## Next-Token Prediction 作為核心任務

在語言模型領域，self-supervised learning 最具代表性的任務是 next-token prediction（下一個 token 預測）。模型根據已知的上下文序列，預測下一個最可能出現的 token。這個看似簡單的任務實際上要求模型理解語法結構、語義關係、世界知識甚至推理能力。GPT 系列模型正是以此任務為基礎進行預訓練，透過海量文字資料習得了廣泛的語言能力與知識。

## 突破資料標註瓶頸

傳統監督學習的最大瓶頸在於標註資料的取得。高品質的人工標註不僅成本高昂，且難以涵蓋所有可能的任務與領域。Self-supervised learning 徹底解決了這個問題——網路上存在近乎無限的未標註文字資料，模型可以直接從中學習。這意味著訓練資料的規模不再受限於標註預算，而是取決於可用的計算資源與資料品質。

## 使規模化成為可能

正因為 self-supervised learning 打破了標註資料的瓶頸，[[foundation-model]] 的規模化才成為現實。研究者可以在數兆個 token 上訓練擁有數千億參數的模型，這在依賴人工標註的時代是完全不可想像的。根據 [[scaling-law]]，模型效能隨著資料量、參數量與計算量的增加而穩定提升，而 self-supervised learning 正是讓這條規模化路徑得以實踐的關鍵技術突破。

## Related

- [[foundation-model]]
- [[tokenization]]
- [[scaling-law]]
- [[transformer-architecture]]
- [[ch01-ai-app-intro-summary]]
