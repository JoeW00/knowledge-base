---
title: "Speculative Decoding"
tags: [inference, decoding, optimization]
created: 2026-04-09
updated: 2026-04-09
---

## Speculative Decoding

Speculative Decoding（推測性解碼）是一種利用較小的草稿模型（draft model）加速大型語言模型推理的技術，其核心思想是用快速的模型先「猜測」多個 token，再由目標模型一次性驗證。

### 核心原理

傳統自迴歸解碼每次只能生成一個 token，受限於 memory bandwidth-bound 的特性，GPU 計算資源大量閒置。Speculative decoding 的運作方式是：首先由草稿模型（draft model）快速生成 K 個候選 token 序列，接著將這些 token 連同原始輸入一起送入目標模型（target model）進行平行驗證。目標模型透過單次前向傳播即可同時驗證所有候選 token，因為這等同於一次 prefill 運算。驗證通過的 token 被接受，遇到第一個不匹配的 token 時，後續所有 token 被拒絕並從該位置重新開始。

### 加速效果

在實務中，speculative decoding 通常能達到 2 到 3 倍的加速比。加速幅度取決於草稿模型與目標模型之間的品質差距——如果草稿模型猜測的準確率高，更多 token 可以被接受，加速效果就越明顯。但即使草稿模型猜測全部錯誤，額外開銷也很有限。

### 草稿模型選擇

草稿模型的選擇至關重要。常見方案包括：使用**同系列的小型模型**（例如用 Llama 7B 為 Llama 70B 生成草稿）、使用極輕量的 **n-gram 模型**從已有上下文中預測、以及 **Medusa** 方案——在目標模型上加裝多個平行預測頭，無需額外草稿模型即可實現類似效果。選擇時需要在草稿模型的速度與準確率之間取得平衡。

### 品質保證

Speculative decoding 的一個關鍵優勢是透過 rejection sampling 機制，可以保證最終輸出的分佈與直接使用目標模型完全一致。這意味著加速不會犧牲任何輸出品質，這是一種「無損加速」技術。

## Related

- [[inference-optimization]]
- [[kv-cache]]
- [[continuous-batching]]
