---
title: "第02章：理解基礎模型 摘要"
source: "raw/articles/ai_engineering_chapters/01-基礎模型概論/第02章-理解基礎模型.md"
tags: [ai-engineering, foundation-model, transformer, training]
created: 2026-04-09
updated: 2026-04-09
---

## 摘要

本章深入探討 foundation model 的核心設計決策。訓練資料以英語為主導，導致低資源語言表現較差且推理成本更高。建模以 Transformer 架構為主軸，說明注意力機制如何解決 seq2seq 瓶頸。模型規模以參數量、訓練 token 數與 FLOPs 衡量，Chinchilla scaling law 指導最佳資源配置。後訓練包含 SFT 與 RLHF/DPO，使模型對齊人類偏好。最後探討 sampling 機制（溫度、top-k、top-p）、推理時計算、結構化輸出，以及不一致性與幻覺問題。

## 重點整理

1. 訓練資料決定模型能力邊界：低資源語言在品質、速度與成本上均處劣勢
2. Chinchilla scaling law 建議訓練 token 數約為參數量的 20 倍
3. 後訓練（SFT + RLHF/DPO）資源消耗僅佔預訓練約 2%，卻能顯著提升可用性
4. Sampling 的機率特性是 AI 創造力的來源，同時也是不一致性與幻覺的根源

## Related

- [[transformer-architecture]]
- [[attention-mechanism]]
- [[rlhf]]
- [[scaling-law]]
- [[sampling-strategy]]
- [[hallucination]]
