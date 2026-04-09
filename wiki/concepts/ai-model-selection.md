---
title: AI 模型選擇行為
tags: [ai-economics, human-ai-interaction, model-selection]
created: 2026-04-09
updated: 2026-04-09
---

## AI 模型選擇行為

AI 模型選擇行為指使用者在不同能力、速度與成本的模型之間做出選擇的決策過程。Anthropic Economic Index 的研究首次以大規模數據揭示了使用者如何在 Haiku、Sonnet 與 Opus 等不同模型類別之間進行取捨。

### 理性選擇的證據

報告數據顯示，使用者的模型選擇行為與任務價值之間存在系統性關聯。在付費 Claude.ai 帳戶中，55% 的電腦與數學相關任務使用 Opus（最高效能模型），而教育類任務僅有 45%。更細緻的分析顯示：

- Software Developer 任務中 34% 使用 Opus，Tutor 任務僅 12%
- 在 Claude.ai 上，任務對應薪資每增加 $10/hr，Opus 使用比例增加 1.5 個百分點
- 在 API 上，這個斜率加倍至 2.8 個百分點，顯示程式化工作流的使用者對模型切換更敏感

### 平台差異

Claude.ai 與 API 使用者在模型選擇上的差異反映了不同的使用情境與動機：

- **Claude.ai 使用者**：可能受使用量限制影響，學會在日常任務使用 Sonnet 以保留 Opus 配額給複雜工作
- **API 使用者**：面對明確的每 token 計價，有更強的經濟動機進行模型切換；程式化工作流讓自動切換更容易實現

### 隱含的「智能需求」概念

這些發現揭示了一個有趣的概念：對 AI 智能的「需求」並非均質的。不同任務對模型能力有不同的最低門檻，而使用者顯然在實踐中學會了辨識這些門檻。這為 AI 服務的差異化定價提供了經濟學基礎，也暗示未來的 AI 產品可能需要更精細的模型路由（model routing）機制，根據任務特性自動選擇最適合的模型。

### 與 learning-by-doing 的關聯

模型選擇能力本身可能是 learning-by-doing 的一個面向。熟練使用者不僅更善於撰寫 prompt，也更擅長判斷何時需要更強大的模型。這種「後設能力」——知道何時需要更多智能——可能是經驗積累帶來的重要技能之一。

## Related

- [[anthropic-economic-index]]
- [[learning-by-doing]]
- [[ai-augmentation-vs-automation]]
- [[model-routing]]
- [[model-selection-workflow]]
- [[scaling-law]]
- [[anthropic-economic-index-learning-curves-summary]]
