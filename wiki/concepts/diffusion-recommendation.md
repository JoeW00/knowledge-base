---
title: 擴散模型推薦 (Diffusion-Based Recommendation)
tags: [recommender-system, diffusion-model, generative-ai, deep-learning]
created: 2026-04-09
updated: 2026-04-09
---

# 擴散模型推薦 (Diffusion-Based Recommendation)

擴散模型推薦將擴散模型的去噪生成過程應用於推薦系統，利用逐步去噪的機制來建模使用者偏好的機率分布，直接生成推薦物品或增強推薦資料。

## 兩種應用方式

**資料增強型**：利用擴散模型的去噪特性來修復與增強推薦系統的輸入資料。例如修復嘈雜的社交網路、補全缺失的模態資訊（圖像、文本）、以及清理含噪的互動紀錄。擴散模型的前向過程（加噪）與反向過程（去噪）天然適合這類資料修復任務。

**目標物品生成型**：直接從使用者偏好的潛在空間中生成推薦物品。不同於傳統方法從固定候選集中選擇，擴散模型探索整個物品分布空間，透過條件導引反向過程（conditional guided reverse process），以使用者偏好與意圖為條件來生成個人化推薦結果。

## 關鍵技術挑戰

**多樣性建模**：傳統模型將使用者偏好編碼為確定性嵌入向量，只能表達有限的偏好面向。DiffDiv 等方法引入多樣性感知的引導學習機制，讓擴散模型能有效捕捉使用者多元的偏好分布。

**最佳化目標設計**：標準擴散模型的訓練目標（重建損失）不一定與推薦目標（點擊率、轉換率）完全對齊。ADRec、PreferDiff 等工作指出需要為推薦場景設計專門的最佳化目標。

**受限生成**：確保生成的推薦物品映射到有效的候選——透過受限詞表解碼、前綴樹搜尋、後處理過濾等策略，保證生成結果的有效性。

## Related

- [[generative-recommendation]]
- [[generative-adversarial-network]]
- [[autoencoder]]
- [[generative-recommendation-survey-summary]]
