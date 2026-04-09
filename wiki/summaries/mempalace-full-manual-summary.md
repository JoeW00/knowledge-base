---
title: MemPalace 完整教學手冊摘要
source: raw/articles/mempalace_report/MemPalace_完整教學手冊.md
tags: [ai-memory, vector-database, chromadb, tutorial, local-first]
created: 2026-04-09
updated: 2026-04-09
---

# MemPalace 完整教學手冊摘要

一份將 MemPalace 報告全 7 章內容整合為單一參考文件的完整教學手冊（v3.0.0），從專案概述到從零實作，適合作為 MemPalace 的全景式入門。

## 結構

手冊依序涵蓋 9 個主題：(1) 專案概述與核心理念（原文逐字儲存、本地優先、零 API）；(2) 宮殿架構（Wing/Room/Hall/Tunnel/Closet 五層結構）；(3) 原始碼模組詳解（21 個 Python 檔案）；(4) 自動儲存 Hook 機制（Save Hook、PreCompact Hook）；(5) 基準測試與重現方法（LongMemEval 96.6%、已知問題誠實聲明）；(6) 整合方式（Claude Code / Gemini CLI / 本地模型）；(7) 從零實作指南；(8) 已知問題與作者聲明；(9) CI/CD 與貢獻指南。

## 作為參考文件的價值

與分章報告相比，完整手冊的優勢在於保留了各章之間的交叉引用脈絡，適合需要快速理解系統全貌的讀者。技術規格：Python 3.9+、依賴 chromadb + pyyaml、Hatchling 建置、MIT 授權。

## 原文路徑

`raw/articles/mempalace_report/MemPalace_完整教學手冊.md`

## Related

- [[ai-memory-system]]
- [[vector-search]]
- [[memory-management]]
- [[mempalace-report-summary]]
- [[mempalace-benchmarks-summary]]
- [[mempalace-integration-summary]]
