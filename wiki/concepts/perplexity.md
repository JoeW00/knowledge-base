---
title: "Perplexity（困惑度）"
tags: [evaluation, nlp, metrics]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Perplexity（困惑度）是衡量語言模型預測下一個 token 不確定程度的核心指標。在數學上，它被定義為交叉熵（cross-entropy）的指數形式。給定一段文本序列，模型會對每個 token 給出一個機率分佈，而 perplexity 就是這些機率的幾何平均倒數。直觀地說，perplexity 的值代表模型在每個位置上「平均在幾個候選 token 之間猶豫」。

## 數值解讀

Perplexity 的值越低，代表模型的預測越準確、越確定。舉例來說，perplexity 為 1 表示模型對每個 token 的預測毫無不確定性（完美預測），而 perplexity 為 100 則表示模型平均在 100 個等機率的候選中選擇。在實務上，現代大型語言模型在通用英文語料上的 perplexity 通常可以降到個位數或十幾的水準。

## 比較限制

需要特別注意的是，不同模型之間的 perplexity 並不總是可以直接比較。主要原因在於不同模型可能使用不同的 tokenizer，導致相同文本被切分成不同數量與粒度的 token。一個使用較細粒度 tokenizer 的模型可能看起來 perplexity 較低，但這不一定代表它的語言理解能力更強。因此在比較時，必須確保使用相同的 tokenizer 或進行正規化處理。

## 延伸應用

除了作為語言模型品質的基本指標外，perplexity 還有多項實用的延伸應用。首先是**資料汙染檢測**：如果模型對某段特定文本的 perplexity 異常低，可能暗示該文本出現在模型的訓練資料中，這對 [[benchmark-contamination]] 的檢測至關重要。其次是**異常文字偵測**：perplexity 異常高的文本片段可能包含亂碼、非自然語言內容或特殊格式錯誤。最後是**訓練資料去重**：透過比較相似文本的 perplexity 分佈，可以識別出重複或近似重複的訓練樣本，提升訓練效率與模型品質。

## Related

- [[lexical-semantic-similarity]]
- [[benchmark-contamination]]
- [[evaluation-pipeline]]
- [[comparative-evaluation]]
