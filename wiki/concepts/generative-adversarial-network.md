---
title: 生成對抗網路 (GAN)
source: raw/articles/deep_learning_from_basic_to_implimentation/05-進階主題與應用/第27章-生成對抗網路.md
tags: [deep-learning, generative-model, gan, creative-ai]
created: 2026-04-09
updated: 2026-04-09
---

# 生成對抗網路 (Generative Adversarial Network, GAN)

生成對抗網路是一種由兩個神經網路互相對抗訓練的生成式架構。**生成器**（Generator）試圖產生以假亂真的資料，**鑑別器**（Discriminator）則試圖區分真實資料與生成器的偽造品。兩者在持續博弈中共同進步，最終生成器能產出與真實資料難以區分的高品質樣本。

## 偽鈔比喻

書中以偽造鈔票的故事直觀解釋 GAN 的運作：Glenn（生成器）在密室中精心製作偽鈔，Dawn（鑑別器）負責將真鈔與偽鈔混合後判別真偽。每當 Dawn 成功識破偽鈔，Glenn 就改進技術；每當 Glenn 的偽鈔騙過 Dawn，Dawn 就提升鑑別力。最終理想狀態是 Dawn 的判斷接近隨機猜測——意味著偽鈔已與真鈔無法區分。

## 訓練機制

GAN 的訓練交替進行兩個階段：固定生成器、訓練鑑別器使其更擅長分辨真偽；再固定鑑別器、訓練生成器使其更擅長欺騙鑑別器。這種對抗動態在理論上收斂到一個 Nash 均衡，但實務中訓練極不穩定，容易出現模式崩塌（mode collapse）——生成器只學會產生少數幾種樣本而非完整的資料分布。

## 應用價值

GAN 的生成能力遠超 VAE 所能達到的品質，特別擅長：圖像風格轉換（「如果梵谷畫大峽谷？」）、超解析度重建、資料增強（為稀缺的訓練資料生成額外樣本）、以及創意設計（傢俱設計、花園規劃）。GAN 也能在不同資料域之間做平滑過渡，混合兩個樣本產生自然的中間形態。

## Related

- [[autoencoder]]
- [[reinforcement-learning]]
- [[backpropagation]]
- [[deep-learning-from-basic-to-implementation-summary]]
