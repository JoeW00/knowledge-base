# 1. 專案概述 — MemPalace 是什麼？

## 一句話總結

MemPalace 是一套**完全本地端運行的 AI 記憶系統**，將你與 AI 的所有對話、決策、程式碼討論，以「宮殿記憶法」的層級結構存入 ChromaDB 向量資料庫，達到目前公開基準測試中最高的 96.6% 召回率（LongMemEval R@5），且**不需要任何 API 金鑰、不上傳任何資料到雲端**。

## 核心理念

| 原則 | 說明 |
|------|------|
| **原文逐字儲存（Verbatim First）** | 不做摘要、不做萃取，保留你的原始對話。96.6% 的成績來自這個「raw mode」 |
| **本地優先（Local First）** | 所有資料留在你的電腦，ChromaDB + SQLite，零雲端依賴 |
| **零 API（Zero API by default）** | 核心功能不需要任何 API key，安裝完就能用 |
| **結構即產品** | Wing → Room → Closet → Drawer 的階層結構帶來 +34% 的檢索提升 |

## 問題場景

你每天與 Claude、ChatGPT、Copilot 對話，累積了大量決策脈絡：
- 「為什麼我們選了 GraphQL？」
- 「上次那個 auth bug 是怎麼修的？」
- 「Kai 上個 sprint 做了什麼？」

**六個月的每日 AI 使用 ≈ 1950 萬 tokens**。這些對話在 session 結束後就消失了。

| 方案 | 載入 Token 數 | 年成本 |
|------|-------------|--------|
| 全部貼上 | 1950 萬（超出任何 context window） | 不可能 |
| LLM 摘要 | ~65 萬 | ~$507/年 |
| **MemPalace wake-up** | **~170 tokens** | **~$0.70/年** |
| **MemPalace + 5 次搜尋** | **~13,500 tokens** | **~$10/年** |

## 版本資訊

- **目前版本**: v3.0.0
- **語言**: Python 3.9+
- **依賴**: `chromadb>=0.5.0,<0.7`, `pyyaml>=6.0`
- **建置系統**: Hatchling
- **授權**: MIT

## 基準測試成績

| 基準測試 | 模式 | 分數 | API 呼叫 |
|----------|------|------|----------|
| LongMemEval R@5 | Raw（純 ChromaDB） | **96.6%** | 零 |
| LongMemEval R@5 | Hybrid + Haiku rerank | **100%**（500/500） | ~500 |
| LoCoMo R@10 | Raw, session level | **60.3%** | 零 |
| Palace structure impact | Wing+room filtering | **+34%** R@10 | 零 |

### 與其他系統比較

| 系統 | LongMemEval R@5 | 需要 API | 費用 |
|------|-----------------|----------|------|
| **MemPalace (hybrid)** | **100%** | 選用 | 免費 |
| Supermemory ASMR | ~99% | 是 | — |
| **MemPalace (raw)** | **96.6%** | **無** | **免費** |
| Mastra | 94.87% | 是（GPT） | API 費用 |
| Mem0 | ~85% | 是 | $19–249/月 |
| Zep | ~85% | 是 | $25/月+ |
