---
title: 反向傳播 (Backpropagation)
source: raw/articles/deep_learning_from_basic_to_implimentation/03-神經網路基礎/第18章-反向傳播.md
tags: [neural-network, training, optimization, deep-learning]
created: 2026-04-09
updated: 2026-04-09
---

# 反向傳播 (Backpropagation)

反向傳播是訓練神經網路的核心演算法，透過計算損失函數對每個權重的梯度，由輸出層向輸入層逐層回傳誤差信號，進而系統性地調整權重以改善預測表現。若沒有反向傳播，現代深度學習幾乎不可能在合理時間內完成訓練。

## 運作原理

訓練流程遵循「前饋→比較→回傳→更新」的循環。網路先對輸入做前饋計算得到預測值，接著用損失函數衡量預測與真實標籤的差距。反向傳播的關鍵在於運用微積分的**鏈式法則**，將整體誤差拆解為每個權重對誤差的個別貢獻量（即梯度），然後沿著梯度方向調整權重，使損失逐步下降。

這個過程之所以高效，是因為每一層只需利用下一層已算好的梯度做局部計算，而不需要對整個網路重新評估，計算量從指數級降為線性級。

## 為什麼重要

反向傳播解決了神經網路長期面臨的「信度分配問題」（credit assignment problem）——當輸出錯誤時，如何公平地判斷數百萬個權重中哪些該負較多責任。在它被廣泛採用之前，神經網路只能停留在淺層結構。正是反向傳播使得深層網路的訓練變得可行，催生了 CNN、RNN 等現代架構的蓬勃發展。

## 實務注意事項

理解反向傳播對於除錯至關重要。常見的**梯度消失**（gradient vanishing）問題——梯度在深層網路中逐層縮小至接近零——會讓網路「拒絕學習」。相對地，**梯度爆炸**則讓權重劇烈震盪。這兩個問題驅動了 ReLU [[activation-function]]、BatchNorm、殘差連接等技術的發明。即使實務上我們使用框架（如 Keras）自動完成反向傳播，理解其機制仍能幫助我們診斷訓練異常並設計更好的網路結構。

## Related

- [[activation-function]]
- [[overfitting-underfitting]]
- [[convolutional-neural-network]]
- [[recurrent-neural-network]]
- [[deep-learning-from-basic-to-implementation-summary]]
