---
title: 卷積神經網路 (CNN)
source: raw/articles/deep_learning_from_basic_to_implimentation/04-深度學習架構與實作/第21章-卷積神經網路.md
tags: [deep-learning, cnn, computer-vision, image-processing]
created: 2026-04-09
updated: 2026-04-09
---

# 卷積神經網路 (Convolutional Neural Network, CNN)

卷積神經網路是一種專門從具有空間或網格結構的資料（尤其是圖像）中提取特徵的深度學習架構。其核心操作「卷積」透過可學習的濾波器在輸入上滑動，自動偵測邊緣、紋理、形狀等由低到高的層次特徵。

## 核心結構

CNN 的典型架構由三種層交替堆疊：**卷積層**使用小型濾波器（kernel）掃描輸入，每個濾波器負責偵測一種特徵模式，輸出稱為特徵圖（feature map）；**池化層**（pooling）對特徵圖進行降維，保留關鍵資訊並增加平移不變性；**全連接層**在最後階段將提取到的高階特徵映射為分類結果或其他任務輸出。

每個卷積層的輸出是三維張量（寬×高×通道數），通道數由該層的濾波器數量決定。多層卷積的級聯讓網路能從像素級的局部特徵逐步組合出物件級的語義理解。

## 為什麼有效

CNN 的成功來自兩個關鍵歸納偏置：**局部連接**假設相鄰像素的關聯性強於遠處像素，因此每個神經元只看一小塊區域；**權重共享**讓同一個濾波器在整張圖上重複使用，大幅減少參數量，同時賦予模型對位移的魯棒性。這兩個特性使 CNN 比全連接網路更適合處理圖像，也更容易訓練。

## 應用範圍

CNN 已在影像分類、物件偵測、醫學影像分析（如皮膚癌檢測）、圖像修復、人臉辨識等領域展現突破性表現。值得注意的是，卷積操作並不限於二維圖像——它同樣適用於一維序列（如自然語言處理中的文本分類）和三維資料（如醫學體積影像），只要資料具備局部相關的網格結構，CNN 就能發揮作用。

## Related

- [[backpropagation]]
- [[activation-function]]
- [[overfitting-underfitting]]
- [[recurrent-neural-network]]
- [[deep-learning-from-basic-to-implementation-summary]]
