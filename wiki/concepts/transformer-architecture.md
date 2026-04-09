---
title: Transformer Architecture
tags: [architecture, deep-learning, foundation-model]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

Transformer 是 2017 年由 Google 研究團隊在 "Attention Is All You Need" 論文中提出的神經網路架構，已成為當前幾乎所有 [[foundation-model]] 的核心骨幹。其核心創新在於完全以 [[attention-mechanism]] 取代傳統的循環結構，實現了高效的平行運算與長距離依賴建模。

## Encoder-Decoder 結構

原始 Transformer 採用 encoder-decoder 結構。Encoder 將輸入序列轉換為連續表徵，decoder 則根據此表徵自迴歸地生成輸出序列。在實際應用中，不同任務偏好不同的架構變體：BERT 採用純 encoder 架構，適合理解任務；GPT 系列採用純 decoder 架構，適合生成任務；T5 則保留完整的 encoder-decoder 結構。當前主流的大語言模型幾乎都採用 decoder-only 架構。

## Self-Attention 的平行處理優勢

相較於 RNN/LSTM 等循環架構必須按序列順序逐步處理，Transformer 的 self-attention 機制允許序列中所有位置同時相互計算注意力權重。這意味著訓練過程可以充分利用 GPU 的平行運算能力，大幅縮短訓練時間。正是這一優勢使得在數兆 token 上訓練數千億參數的模型成為可能，直接推動了 foundation model 的規模化發展。

## 位置編碼

由於 self-attention 本身不包含序列順序資訊，Transformer 需要額外的位置編碼（positional encoding）來注入位置訊號。早期使用固定的正弦位置編碼，後來 RoPE（Rotary Position Embedding）成為主流方案，它能更好地泛化到訓練時未見過的序列長度，支援模型處理更長的上下文窗口。

## 自迴歸解碼的限制

Decoder 在生成時採用自迴歸方式，每次僅產生一個 token，且每個 token 的生成都依賴於所有先前生成的 token。這導致推理速度受限於序列長度，成為部署時的主要效能瓶頸。各種加速技術如 KV cache、speculative decoding 等正試圖緩解此問題。

## SSM/Mamba 等替代架構

近年來，State Space Model（SSM）及其代表 Mamba 架構作為 Transformer 的替代方案受到關注。SSM 以線性複雜度處理序列，在長序列場景下具有計算效率優勢。然而，Transformer 在大多數基準測試上仍然表現最佳，且其生態系統最為成熟，短期內仍是 foundation model 的主流選擇。

## Related

- [[attention-mechanism]]
- [[foundation-model]]
- [[self-supervised-learning]]
- [[scaling-law]]
- [[sampling-strategy]]
- [[ch02-understanding-foundation-models-summary]]
