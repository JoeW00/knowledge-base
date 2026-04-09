---
title: "Benchmark 資料汙染（Benchmark Contamination）"
tags: [evaluation, training, data-quality]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

Benchmark 資料汙染是指模型的訓練資料中不慎包含了評估資料集的內容，導致模型在該 benchmark 上的分數被人為抬高，無法真實反映模型的實際能力。這個問題在大型語言模型時代變得尤其嚴重，因為模型的訓練語料通常來自大規模網路爬取，而許多公開 benchmark 的題目與答案也散佈在網路上。

## 發生原因

資料汙染的根本原因在於訓練資料的規模與來源難以完全控制。現代 LLM 的訓練語料動輒數兆個 token，涵蓋網路上的大量公開文本。當 benchmark 的測試集被發佈在學術論文、部落格、GitHub 或論壇等平台上，這些內容很可能在爬取過程中被納入訓練資料。此外，部分 benchmark 的題目設計過於固定，缺乏多樣性，使得即使是間接接觸也可能讓模型「記住」答案模式。

## 檢測方法

檢測資料汙染的常見方法包括：**[[perplexity]] 異常檢測**——如果模型對某些測試樣本的 perplexity 異常低（遠低於同類型文本的平均值），可能暗示該樣本出現在訓練資料中。**Canary string 方法**——在測試集中植入獨特的標記字串，然後檢查模型是否能生成這些字串。**成員推理攻擊（membership inference）**——透過統計方法判斷特定樣本是否為訓練集的一部分。

## 影響

資料汙染對公開排行榜的可信度造成嚴重衝擊。當多個模型的訓練資料都不同程度地包含 benchmark 內容時，排行榜上的排名可能無法反映真實的模型能力差距。這使得從業者在進行 [[model-selection-workflow]] 時，不能完全依賴公開 benchmark 的結果。

## 因應策略

為了降低資料汙染的影響，業界採用多種策略。首先是**建構私有評估集**，不公開測試題目與答案，確保不會被爬取。其次是**定期更新 benchmark**，讓舊題目退役、新題目加入，降低被記憶的風險。最後，在模型評估時應結合多種方法（包括 [[comparative-evaluation]] 等不依賴固定題庫的方式），而非僅依賴單一 benchmark 的分數。

## Related

- [[perplexity]]
- [[model-selection-workflow]]
- [[comparative-evaluation]]
- [[evaluation-pipeline]]
- [[evaluation-driven-development]]
