---
title: Scaling Law
tags: [training, foundation-model, research]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

Scaling law（縮放定律）是描述模型規模與效能之間關係的經驗法則。研究表明，隨著模型參數量、訓練資料量與計算量的增加，模型在各項基準測試上的表現會以可預測的方式持續提升。這些定律為 [[foundation-model]] 的訓練規劃提供了重要的理論指引，幫助研究團隊在有限預算下做出最佳資源配置決策。

## 三大衡量指標

衡量模型規模有三個核心指標。第一是參數量（parameters），即模型中可學習權重的數量，從早期的數億到如今的數千億甚至兆級。第二是訓練 token 數（training tokens），即模型在訓練過程中處理的 token 總量，反映了模型接觸的資料規模。第三是 FLOPs（floating-point operations），即訓練所需的浮點運算總量，直接關聯計算成本。這三個指標相互關聯，共同決定了模型的最終能力。

## Chinchilla Scaling Law

2022 年 DeepMind 發表的 Chinchilla 研究是 scaling law 領域的里程碑。該研究指出，在固定計算預算下，最佳策略是讓訓練 token 數約為參數量的 20 倍（20:1 token-to-parameter ratio）。這挑戰了先前「更大模型總是更好」的假設——許多大模型實際上是「訓練不足」的，它們擁有過多參數但未接觸足夠的訓練資料。Chinchilla 以 700 億參數但更充分的訓練，在多項基準上超越了 2800 億參數的 Gopher。

## Compute-Optimal Training

Compute-optimal training 的概念源自 Chinchilla scaling law，核心思想是在給定的計算預算下找到參數量與訓練資料量的最佳平衡點。實務上，這意味著研究團隊需要根據可用的 GPU 時數與資料量，計算出最具成本效益的模型規模。然而，部署階段的推理成本也是重要考量——較小但訓練更充分的模型在推理時更為經濟，這也是為何許多實際部署偏好中等規模模型的原因。

## Scaling 是否有極限的爭論

Scaling law 是否會持續有效是 AI 社群中最受關注的爭論之一。樂觀派認為，只要持續增加計算資源與資料，模型能力將不斷提升，最終通往通用人工智慧。悲觀派則指出，高品質訓練資料正在耗盡，且某些能力（如深度推理）可能無法單靠規模提升。此外，推理時計算（test-time compute）作為補充路徑的興起，暗示著單純的預訓練規模化可能並非唯一的能力提升途徑。

## Related

- [[foundation-model]]
- [[self-supervised-learning]]
- [[transformer-architecture]]
- [[tokenization]]
- [[ch02-understanding-foundation-models-summary]]
- [[generative-recommendation]]
- [[large-recommendation-model]]
- [[generative-recommendation-survey-summary]]
