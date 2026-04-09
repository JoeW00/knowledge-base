---
title: "第04章：評估 AI 系統 摘要"
source: "raw/articles/ai_engineering_chapters/02-評估與品質/第04章-評估AI系統.md"
tags: [ai-engineering, evaluation, system-design]
created: 2026-04-09
updated: 2026-04-09
---

## 摘要

本章聚焦於如何在具體應用情境中評估模型，涵蓋三大主題：評估標準的定義（領域能力、生成品質、指令遵循、成本延遲）、模型選擇流程（自建 vs. 購買、公開 benchmark 與排行榜的使用及其資料汙染風險），以及評估流水線的設計（元件級評估、評估指南制定、資料標注與自助抽樣驗證）。作者提出「評估驅動開發」理念，強調在建構系統之前先定義好成功標準。

## 重點整理

1. **[[evaluation-driven-development]]**——在建構 AI 應用之前先定義好評估標準，類似軟體工程中的測試驅動開發，確保開發方向明確。
2. **[[factual-consistency]] 驗證分為局部與全局兩種**——局部驗證對照給定上下文（如 RAG 場景），全局驗證對照公開知識庫。
3. **公開 benchmark 存在嚴重的 [[benchmark-contamination]] 問題**——模型訓練資料可能不慎包含測試集，導致排行榜分數虛高。
4. **[[model-selection-workflow]] 需在品質、成本、延遲間取得平衡**——不同應用場景對三者的優先順序不同。
5. **[[evaluation-pipeline]] 本身也需要被評估**——透過自助抽樣等統計方法驗證評估結果的可靠性。

## Related

- [[evaluation-driven-development]]
- [[factual-consistency]]
- [[benchmark-contamination]]
- [[instruction-following]]
- [[model-selection-workflow]]
- [[evaluation-pipeline]]
