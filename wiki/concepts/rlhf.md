---
title: RLHF（基於人類反饋的強化學習）
tags: [training, alignment, foundation-model]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

RLHF（Reinforcement Learning from Human Feedback，基於人類反饋的強化學習）是一種後訓練技術，透過人類偏好資料來微調 [[foundation-model]]，使其生成更符合人類期望的回應。RLHF 是 ChatGPT 等對話模型能夠安全、有用且自然地回應使用者的關鍵技術。

## SFT → 獎勵模型 → PPO 三階段流程

經典 RLHF 包含三個階段。第一階段是 Supervised Fine-Tuning（SFT），使用人工撰寫的高品質對話範例對預訓練模型進行微調，使其初步具備指令遵循能力。第二階段是訓練獎勵模型（Reward Model），讓人類標註者對模型生成的多個回應進行排序，用排序資料訓練一個能預測人類偏好分數的模型。第三階段使用 Proximal Policy Optimization（PPO）強化學習演算法，以獎勵模型的評分作為回饋信號，進一步最佳化語言模型的生成策略。

## DPO 作為簡化替代

Direct Preference Optimization（DPO）是近年來提出的 RLHF 簡化方案。DPO 跳過了訓練獎勵模型與 PPO 的步驟，直接利用人類偏好資料（偏好回應與非偏好回應的配對）來最佳化語言模型。DPO 的訓練過程更穩定、更易實作，且在多項基準測試上表現與傳統 RLHF 相當甚至更好，因此受到越來越多團隊的採用。

## Constitutional AI（RLAIF）

Anthropic 提出的 Constitutional AI 將人類反饋進一步替換為 AI 反饋（RLAIF，Reinforcement Learning from AI Feedback）。系統預先定義一組「憲法原則」，由 AI 模型自身根據這些原則評估並改進回應。這種方法減少了對人類標註者的依賴，使對齊過程更具可擴展性，同時仍能維持高品質的對齊效果。

## 資源效率與效果

後訓練（包含 SFT 與 RLHF/DPO）所需的計算資源僅佔預訓練的約 2%，但對模型可用性的提升卻極為顯著。未經後訓練的模型只會「續寫文字」，經過後訓練後則能理解並遵循指令、拒絕有害請求、提供結構化回應。這種投入產出比使後訓練成為 foundation model 開發中最具性價比的環節。

## 對模型安全性與對齊的意義

RLHF 與其變體是當前 AI 對齊（alignment）的主要實踐手段。透過人類反饋，模型學會辨識並拒絕有害、偏見或不當的請求，同時保持對合理請求的有用回應。然而，對齊的穩健性仍是開放議題——jailbreak 攻擊等技術持續挑戰現有的安全防線。

## Related

- [[foundation-model]]
- [[self-supervised-learning]]
- [[scaling-law]]
- [[hallucination]]
- [[ch02-understanding-foundation-models-summary]]
