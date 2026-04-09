---
title: MemPalace 整合方式與使用範例摘要
source: raw/articles/mempalace_report/06_integration_examples.md
tags: [ai-memory, mcp, agent, integration, hooks]
created: 2026-04-09
updated: 2026-04-09
---

# MemPalace 整合方式與使用範例摘要

本章展示 MemPalace 與各種 AI 工具的整合方式，從全自動到手動，涵蓋四類場景。

## 整合矩陣

| 工具 | 整合方式 | 自動化程度 |
|------|----------|-----------|
| Claude Code | MCP Server + Hooks | 全自動 |
| Gemini CLI | MCP Server + Hooks | 全自動 |
| ChatGPT / Cursor | MCP Server | 半自動 |
| 本地模型 | CLI / Python API | 手動 |

## Claude Code 整合（核心路徑）

透過 `claude mcp add mempalace` 註冊 MCP Server，Claude Code 即獲得 19 個 MCP 工具，使用者正常提問（如「上個月我們對 auth 做了什麼決定？」），Claude 自動呼叫 `mempalace_search` 取得結果。配合 Save Hook 與 PreCompact Hook 設定在 `.claude/settings.local.json`，可實現對話結束時自動存入記憶、context 壓縮前自動備份。

## 本地模型整合

三種方式：(1) `mempalace wake-up` 生成 ~170 tokens 的 context 貼入 system prompt；(2) CLI 搜尋結果手動餵入 prompt；(3) Python API 程式化整合。這讓 [[local-llm-inference]] 場景也能使用持久記憶。

## 原文路徑

`raw/articles/mempalace_report/06_integration_examples.md`

## Related

- [[ai-memory-system]]
- [[memory-management]]
- [[ai-agent]]
- [[local-llm-inference]]
- [[mempalace-report-summary]]
