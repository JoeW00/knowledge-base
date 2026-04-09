---
title: 過擬合與欠擬合 (Overfitting & Underfitting)
source: raw/articles/deep_learning_from_basic_to_implimentation/02-機器學習基礎/第09章-過擬合與欠擬合.md
tags: [machine-learning, training, regularization, generalization]
created: 2026-04-09
updated: 2026-04-09
---

# 過擬合與欠擬合 (Overfitting & Underfitting)

過擬合與欠擬合是機器學習中最核心的兩難問題，本質上反映了模型在「記住訓練資料的特殊細節」與「學到可泛化的通用規則」之間的平衡。

## 過擬合 (Overfitting)

當模型在訓練資料上表現極好，但在未見過的新資料上表現糟糕時，就是過擬合。書中以婚禮認人的比喻生動說明：我們記住了 Walter 的海象鬍子和 Erin 的耳環，卻在遇到另一位蓄鬍子的 Bob 時叫錯名字。問題不在於我們沒學習，而在於我們學到的特徵太過具體，無法推廣到新情境。

過擬合的根源是模型容量相對於資料量過大——模型有足夠的參數去「死記」每一筆訓練樣本的細節，而非歸納出底層規律。

## 欠擬合 (Underfitting)

相反地，當模型對訓練資料本身的學習都不充分、連訓練集上的表現都很差時，就是欠擬合。通常原因是模型容量太小、訓練時間不足、或特徵工程不當。

## 正則化 (Regularization)

控制過擬合的一系列技術統稱為**正則化**，常見方法包括：L1/L2 權重懲罰（限制權重大小）、Dropout（隨機關閉部分神經元）、早停法（在驗證誤差開始上升時停止訓練）、資料增強（擴充訓練集的多樣性）。

與過擬合密切相關的概念是**偏差-方差權衡**（bias-variance tradeoff）：欠擬合模型有高偏差、低方差；過擬合模型有低偏差、高方差。理想的模型在兩者之間取得最佳平衡點。這個權衡貫穿所有模型選擇與超參數調整的決策過程。

## Related

- [[backpropagation]]
- [[convolutional-neural-network]]
- [[activation-function]]
- [[deep-learning-from-basic-to-implementation-summary]]
