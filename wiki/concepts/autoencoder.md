---
title: 自編碼器 (Autoencoder)
source: raw/articles/deep_learning_from_basic_to_implimentation/05-進階主題與應用/第25章-自編碼器.md
tags: [deep-learning, generative-model, dimensionality-reduction, unsupervised-learning]
created: 2026-04-09
updated: 2026-04-09
---

# 自編碼器 (Autoencoder)

自編碼器是一種學習將輸入壓縮為低維表徵、再從該表徵重建原始輸入的神經網路架構。其訓練目標是讓輸出儘可能接近輸入，迫使網路在壓縮瓶頸處學到資料最關鍵的特徵。

## 結構與原理

自編碼器由兩部分組成：**編碼器**（encoder）將高維輸入映射到低維潛在空間（latent space），**解碼器**（decoder）從潛在表徵重建輸出。中間的瓶頸層維度遠小於輸入維度，強制網路丟棄冗餘資訊，只保留最具代表性的特徵。這類似於 JPG 壓縮圖片或 MP3 壓縮音樂，但自編碼器是從資料本身學習壓縮策略，而非使用手工設計的演算法。

自編碼器的壓縮是**有損的**且**資料相依的**——用流行音樂訓練的編碼器壓縮交響樂效果必然不佳。

## 主要用途

實務上自編碼器有兩大應用：**去噪**（denoising）——訓練時刻意在輸入加入雜訊，讓網路學會還原乾淨資料；**降維**——壓縮後的潛在表徵可作為下游模型的輸入特徵，通常比原始資料更有效率且能帶來更好的訓練結果。

## 變分自編碼器 (VAE)

**變分自編碼器**（Variational Autoencoder）是自編碼器的重要變體。VAE 不是將輸入映射到單一潛在向量，而是映射到一個**機率分布**（通常是高斯分布的均值與方差）。這使得我們能從潛在空間中隨機取樣，再透過解碼器**生成**全新的、與訓練資料風格相似但不完全相同的資料。VAE 還支援在兩個樣本的潛在向量之間做平滑插值，產生自然的漸變過渡效果。

## Related

- [[generative-adversarial-network]]
- [[backpropagation]]
- [[overfitting-underfitting]]
- [[deep-learning-from-basic-to-implementation-summary]]
