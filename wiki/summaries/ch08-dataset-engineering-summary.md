---
title: "第08章：資料集工程 摘要"
source: "raw/articles/ai_engineering_chapters/04-模型訓練與資料/第08章-資料集工程.md"
tags: [ai-engineering, data, training]
created: 2026-04-09
updated: 2026-04-09
---

## 摘要

本章闡述「模型品質取決於訓練資料品質」的核心理念。資料策展須兼顧三大要素：data quality（六項特徵）、data coverage（多樣性維度）、資料量。深入探討 data synthesis 技術（規則生成、AI 驅動合成），同時警示 synthetic data 的局限（model collapse、表層模仿）。最後介紹 model distillation 與資料處理實務。

## 重點整理

1. 少量高質量資料優於大量含噪資料（LIMA 僅用 1,000 條資料即可媲美 GPT-4）
2. 資料多樣性與品質同等重要
3. AI 合成資料需經嚴格驗證，未經驗證的合成資料風險極高
4. 遞迴使用 AI 生成資料訓練可能導致 model collapse
5. 花 15 分鐘人工檢查資料，往往能避免後續數小時麻煩

## Related

- [[data-quality]]
- [[synthetic-data]]
- [[model-distillation]]
- [[data-curation]]
- [[model-collapse]]
- [[data-augmentation]]
