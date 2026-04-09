---
title: 循環神經網路 (RNN)
source: raw/articles/deep_learning_from_basic_to_implimentation/04-深度學習架構與實作/第22章-循環神經網路.md
tags: [deep-learning, rnn, lstm, sequence-modeling, nlp]
created: 2026-04-09
updated: 2026-04-09
---

# 循環神經網路 (Recurrent Neural Network, RNN)

循環神經網路是一種專為處理序列資料而設計的深度學習架構。與前饋網路將每筆樣本視為獨立個體不同，RNN 在處理序列中的每個時間步時會保留來自先前步驟的「記憶」，使其能夠捕捉資料中的時序依賴關係。

## 核心機制

RNN 的基本結構是在隱藏層中引入一個**迴圈連接**：當前時間步的隱藏狀態不僅取決於當前輸入，還取決於上一個時間步的隱藏狀態。這個遞迴結構在概念上等同於將網路沿時間軸「展開」成一個非常深的前饋網路，每一層共享相同的權重。

然而，基礎 RNN 在實務中難以學習長距離依賴，因為梯度在時間步間傳播時會快速消失或爆炸。為此，**LSTM**（Long Short-Term Memory）引入了門控機制（遺忘門、輸入門、輸出門），讓網路能選擇性地記住或遺忘資訊，有效緩解梯度消失問題。LSTM 已成為 RNN 的事實標準實作方式。

## 應用領域

RNN 擅長處理任何具有順序意義的資料：語音辨識（聲波→文字）、自然語言翻譯（序列到序列）、時間序列預測（股價、溫度）、音樂生成等。一個訓練好的 RNN 不僅能理解序列，還能**生成**新序列——從幾個音符延伸出完整旋律，或根據風格續寫文本。

值得注意的是，在自然語言處理領域，RNN 的主導地位已逐漸被 [[transformer-architecture]] 的注意力機制所取代，後者能更高效地建模長距離依賴。然而 RNN 在資源受限的即時序列處理場景中仍具優勢。

## Related

- [[backpropagation]]
- [[convolutional-neural-network]]
- [[activation-function]]
- [[transformer-architecture]]
- [[deep-learning-from-basic-to-implementation-summary]]
