---
title: MemPalace AI 記憶系統報告摘要
source: raw/articles/mempalace_report
tags: [ai-memory, vector-database, chromadb, local-first, agent]
created: 2026-04-09
updated: 2026-04-09
---

# MemPalace AI 記憶系統報告摘要

MemPalace 是一套完全本地端運行的 AI 記憶系統，以「宮殿記憶法」的層級結構將 AI 對話存入 ChromaDB 向量資料庫，在 LongMemEval 基準測試中達到 96.6% 召回率（raw 模式，零 API），hybrid 模式更達 100%。報告共 7 章加一份完整教學手冊。

## 核心架構

系統以五層結構組織記憶：**Wing**（人物或專案的命名空間）→ **Room**（具體主題）→ **Hall**（連接同一 Wing 內相關 Room 的記憶類型通道，分 facts/events/discoveries/preferences/advice 五種）→ **Closet/Drawer**（細粒度存儲）。不同 Wing 中同名 Room 之間以 **Tunnel** 自動連接，實現跨域檢索。這種結構化設計帶來 +34% 的檢索精度提升。

核心設計原則是「原文逐字儲存（Verbatim First）」——不做摘要或萃取，直接保存原始對話，以此保留最大的資訊量。資料攝取支援 Claude Code JSONL、Claude.ai JSON、ChatGPT 等 5 種格式。

## 關鍵論點

1. **本地優先 + 零 API 即可達高效能**：96.6% 召回率完全在本地完成，不需要任何 LLM API，年成本近乎零
2. **結構即檢索品質**：Wing/Room 的層級過濾比扁平向量搜尋提升 34%，證明 [[vector-search]] 的元資料過濾策略的重要性
3. **AAAK 壓縮**：多層壓縮機制（L0 wake-up context ~170 tokens → L1/L2 逐級展開），讓 AI 能用極少 token 載入記憶上下文
4. **與 Claude Code hooks 深度整合**：透過 pre/post-tool hooks 在開發流程中自動記錄與檢索記憶

## 原文路徑

`raw/articles/mempalace_report/`（01-07 章節 + 完整教學手冊）

## Related

- [[ai-memory-system]]
- [[vector-search]]
- [[memory-management]]
- [[embedding]]
