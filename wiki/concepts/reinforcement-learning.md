---
title: 強化學習 (Reinforcement Learning)
source: raw/articles/deep_learning_from_basic_to_implimentation/05-進階主題與應用/第26章-強化學習.md
tags: [machine-learning, reinforcement-learning, agent, reward]
created: 2026-04-09
updated: 2026-04-09
---

# 強化學習 (Reinforcement Learning)

強化學習是一種訓練範式，智能體（agent）在環境中採取行動，透過環境回饋的獎勵信號逐步學習最佳策略。它介於監督學習（有明確標籤）與無監督學習（無任何回饋）之間——我們不告訴系統「正確答案」是什麼，只告訴它某個行動的結果「好」或「不好」。

## 核心框架

強化學習的四個基本元素：**智能體**在每個時間步觀察環境**狀態**，選擇一個**行動**，環境轉移到新狀態並回傳一個**獎勵**信號。智能體的目標是學習一個**策略**（policy），使得長期累積獎勵最大化。

關鍵挑戰在於**延遲獎勵**——許多場景中，行動的好壞要等到很久之後才知道（例如棋局要到終盤才分勝負），智能體必須學會將最終結果歸因到過程中的每步決策。此外，智能體需在**探索**（嘗試未知行動）與**利用**（重複已知的好行動）之間取得平衡。

## 典型應用

書中以電梯調度為例：最佳策略取決於建築物的人流模式，且模式會隨時段、季節變化。沒有固定的「正確答案」，只有相對更好的策略。類似地，農作物種植（播種深度、灌溉頻率）、機器人步態學習（讓雙足機器人學會走路）、遊戲 AI（圍棋、電玩）都是強化學習的理想場景。

強化學習的價值在於處理**不確定性**與**動態環境**——即使其他參與者做出意料之外的行為，或環境條件突然改變，訓練好的策略仍能維持合理的表現。

## Related

- [[generative-adversarial-network]]
- [[backpropagation]]
- [[overfitting-underfitting]]
- [[deep-learning-from-basic-to-implementation-summary]]
