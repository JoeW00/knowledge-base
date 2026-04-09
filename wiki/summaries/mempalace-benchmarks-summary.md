---
title: MemPalace 基準測試與重現方法摘要
source: raw/articles/mempalace_report/05_benchmarks.md
tags: [ai-memory, evaluation, benchmark, chromadb]
created: 2026-04-09
updated: 2026-04-09
---

# MemPalace 基準測試與重現方法摘要

本章詳述 MemPalace 的 4 套基準測試框架及其重現方法，並包含作者對自身宣稱的誠實修正。

## 測試框架

核心基準為 **LongMemEval**（500 個問答對）：raw 模式（純 ChromaDB 向量搜尋，零 API）達 96.6% R@5；hybrid 模式（向量搜尋 + Claude Haiku rerank）達 100%。另有 LoCoMo（對話記憶理解，raw session level 60.3% R@10）、ConvoMem（Salesforce 7.5 萬 QA，6 個類別）、MemBench（ACL 2025 學術基準）。

hybrid 模式的 rerank 機制是先用 ChromaDB 向量搜尋取得候選結果，再用 LLM 重新排序——本質上就是 [[rag]] 中 reranking 策略的應用。

## 誠實聲明（重要）

作者公開修正了 5 項過度宣稱：(1) AAAK token 估算有誤，小規模下不省 token；(2)「30x 無損壓縮」實為有損（84.2% vs. raw 96.6%）；(3)「+34% palace boost」是標準 ChromaDB metadata filtering，非創新機制；(4)「矛盾偵測」模組存在但未整合；(5)「100% with Haiku rerank」結果檔案存在但 pipeline 未公開。這種自我修正的透明度值得肯定，也提醒讀者在評估任何系統宣稱時應檢視重現方法。

## 原文路徑

`raw/articles/mempalace_report/05_benchmarks.md`

## Related

- [[ai-memory-system]]
- [[vector-search]]
- [[evaluation-pipeline]]
- [[mempalace-report-summary]]
