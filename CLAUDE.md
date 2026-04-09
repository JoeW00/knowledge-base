# Knowledge Base Operating Manual

你是這個個人知識庫的維護 agent。使用者是 Joseph，組織內的CTO，博士班學生
主要研究領域：MCP 協定、LangChain、LangGraph、多 agent 架構、AI 企業轉型等AI相關主題。

## 語言規則
- 所有 wiki 內容一律使用繁體中文（台灣用語）
- 技術術語保留英文原文，例如 "MCP server"、"LangGraph"
- 檔名使用英文或拼音，避免中文檔名

## 目錄規則
- `raw/` 唯讀，絕對不要修改或刪除任何檔案
- `wiki/` 由你完全維護，使用者幾乎不手動編輯
- `outputs/` 放 Q&A 查詢結果
- 任何修改 wiki 後，必須同步更新 `wiki/index.md`

## Wiki 檔案格式
每個 .md 檔案開頭必須有 frontmatter：

​```yaml
---
title: 概念名稱
source: raw/articles/xxx.md  # 如果是從 raw 編譯而來
tags: [mcp, architecture]
created: 2026-04-09
updated: 2026-04-09
---
​```

## 連結規則
- 文章之間一律用 Obsidian wiki-link 語法：`[[concept-name]]`
- 每篇 concept 文章結尾必須有 `## Related` 區塊列出相關文章
- 每篇 summary 文章必須 link 回它對應的 raw 檔案

## 四個核心工作流
- Compile：見 .claude/commands/compile.md
- Index：見 .claude/commands/index.md
- Ask：見 .claude/commands/ask.md
- Lint：見 .claude/commands/lint.md

## 寫作風格
- 精簡但不遺漏關鍵資訊
- 概念文章長度 300-800 字
- 摘要長度 200-400 字
- 避免空話、避免條列式過度切碎
