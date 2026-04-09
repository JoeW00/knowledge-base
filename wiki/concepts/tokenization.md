---
title: Tokenization
tags: [nlp, foundation-model, preprocessing]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Tokenization（分詞）是將原始文字拆分為 token 的過程，是所有語言模型處理文字的第一步。Token 是模型實際處理的最小單位，可以是一個完整的單詞、一個子詞片段，甚至是單一字元。Tokenization 的設計直接影響模型的語言覆蓋能力、推理效率與成本結構。

## Subword Tokenization

現代 [[foundation-model]] 普遍採用 subword tokenization 方法，其中最具代表性的是 Byte Pair Encoding（BPE）。BPE 從字元層級出發，透過統計共現頻率反覆合併最常見的字元對，逐步建構出 vocabulary（詞彙表）。這種方法在詞彙表大小與文字覆蓋範圍之間取得平衡——常見詞以完整 token 表示，罕見詞則被拆分為多個子詞片段。其他常見方法包括 WordPiece 與 SentencePiece 等。

## Vocabulary 大小的影響

Vocabulary 的大小是一個關鍵設計決策。較大的 vocabulary 能將更多常見詞彙編碼為單一 token，提高編碼效率，但也增加了模型的嵌入層參數量。較小的 vocabulary 則導致文字被拆分為更多 token，增加序列長度與推理成本。典型的 foundation model vocabulary 規模在數萬到十幾萬之間，例如 GPT-4 使用約 100,000 個 token 的詞彙表。

## 多語言 Tokenization 的挑戰

多語言 tokenization 是當前最具挑戰性的問題之一。由於訓練語料以英語為主導，BPE 演算法在合併過程中自然偏向英語的字元組合。結果是低資源語言（如中文、泰文、阿拉伯文等）的相同語義內容往往需要更多 token 才能表達，效率差異可達十倍之多。這不僅影響模型對這些語言的理解品質，更直接導致推理成本的增加。

## 對推理成本的影響

在 MaaS 的計價模式下，API 費用通常以 token 數量計算。因此 tokenization 效率直接關聯使用成本——同樣的語義內容，tokenization 效率較低的語言需要消耗更多 token，產生更高的費用。此外，更長的 token 序列也意味著更多的計算量與更高的延遲，這對即時應用的使用者體驗有著直接的影響。

## Related

- [[foundation-model]]
- [[self-supervised-learning]]
- [[transformer-architecture]]
- [[attention-mechanism]]
- [[ch02-understanding-foundation-models-summary]]
