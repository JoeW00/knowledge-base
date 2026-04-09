# 6. 整合方式與使用範例

## 整合方式一覽

MemPalace 支援多種 AI 工具的整合：

| 工具 | 整合方式 | 自動化程度 |
|------|----------|-----------|
| Claude Code | MCP Server + Hooks | 全自動 |
| Gemini CLI | MCP Server + Hooks | 全自動 |
| ChatGPT / Cursor | MCP Server | 半自動 |
| 本地模型（Llama, Mistral） | CLI / Python API | 手動 |

---

## Claude Code 整合

### 步驟 1：註冊 MCP Server

```bash
claude mcp add mempalace -- python -m mempalace.mcp_server
```

完成後，Claude Code 自動獲得 19 個 MCP 工具。你只需正常提問：

> *「上個月我們對 auth 做了什麼決定？」*

Claude 自動呼叫 `mempalace_search`，取得原文結果，回答你。

### 步驟 2：設定自動儲存 Hook

在 `.claude/settings.local.json` 中加入 Save Hook 和 PreCompact Hook（見第 4 章）。

---

## Gemini CLI 整合

### 步驟 1：安裝

```bash
git clone https://github.com/milla-jovovich/mempalace.git
cd mempalace
python3 -m venv .venv
.venv/bin/pip install -e .
```

### 步驟 2：初始化 Palace

```bash
.venv/bin/python3 -m mempalace init .
```

### 步驟 3：註冊 MCP Server

```bash
gemini mcp add mempalace /absolute/path/to/mempalace/.venv/bin/python3 -m mempalace.mcp_server --scope user
```

### 步驟 4：設定 Hook

在 `~/.gemini/settings.json` 中加入 `PreCompress` hook：

```json
{
  "hooks": {
    "PreCompress": [{
      "matcher": "*",
      "hooks": [{
        "type": "command",
        "command": "/absolute/path/to/mempalace/hooks/mempal_precompact_hook.sh"
      }]
    }]
  }
}
```

---

## 本地模型整合

### 方法 1：Wake-up 命令

```bash
mempalace wake-up > context.txt
# 將 context.txt 貼入你的本地模型 system prompt
```

這會給你的本地模型 ~170 tokens 的關鍵事實（可選 AAAK 格式）。

### 方法 2：CLI 搜尋 + 手動餵入

```bash
mempalace search "auth decisions" > results.txt
# 將 results.txt 放入你的 prompt
```

### 方法 3：Python API

```python
from mempalace.searcher import search_memories
results = search_memories("auth decisions", palace_path="~/.mempalace/palace")
# 注入到本地模型的 context
```

---

## 實際使用情境

### 情境 1：Solo 開發者 — 跨多專案

```bash
# 挖掘每個專案的對話
mempalace mine ~/chats/orion/  --mode convos --wing orion
mempalace mine ~/chats/nova/   --mode convos --wing nova
mempalace mine ~/chats/helios/ --mode convos --wing helios

# 六個月後：「為什麼這個專案用 Postgres？」
mempalace search "database decision" --wing orion
# → "選了 Postgres 而非 SQLite，因為 Orion 需要並發寫入且資料集會超過 10GB。
#    2025-11-03 決定。"

# 跨專案搜尋
mempalace search "rate limiting approach"
# → 同時找到 Orion 和 Nova 中的做法，顯示差異
```

### 情境 2：團隊主管 — 管理產品

```bash
# 挖掘 Slack 匯出和 AI 對話
mempalace mine ~/exports/slack/ --mode convos --wing driftwood
mempalace mine ~/.claude/projects/ --mode convos

# 「Soren 上個 sprint 做了什麼？」
mempalace search "Soren sprint" --wing driftwood
# → 14 個 closet：OAuth 重構、dark mode、component library 遷移

# 「誰決定用 Clerk？」
mempalace search "Clerk decision" --wing driftwood
# → "Kai 推薦 Clerk 而非 Auth0 — 定價 + 開發體驗。
#    團隊 2026-01-15 同意。Maya 負責遷移。"
```

### 情境 3：挖掘前拆分大檔案

```bash
mempalace split ~/chats/                      # 拆成單 session 檔案
mempalace split ~/chats/ --dry-run            # 預覽
mempalace split ~/chats/ --min-sessions 3     # 只拆含 3+ session 的檔案
```

---

## Specialist Agent（專家代理）

在 `~/.mempalace/agents/` 下建立 JSON 設定：

```
~/.mempalace/agents/
  ├── reviewer.json       # 程式品質、模式、bug
  ├── architect.json      # 設計決策、取捨
  └── ops.json            # 部署、事件、基礎設施
```

你的 CLAUDE.md 只需一行：
```
You have MemPalace agents. Run mempalace_list_agents to see them.
```

每個 Agent：
- 有特定焦點（專注領域）
- 有自己的日記（AAAK 格式，跨 session 持久化）
- 建立專業知識（閱讀自己的歷史記錄）

```python
# Agent 在 code review 後寫日記
mempalace_diary_write("reviewer",
    "PR#42|auth.bypass.found|missing.middleware.check|pattern:3rd.time.this.quarter|★★★★")

# Agent 讀取自己的歷史
mempalace_diary_read("reviewer", last_n=10)
```

---

## 19 個 MCP 工具完整清單

### Palace 讀取（7 個）
| 工具 | 功能 |
|------|------|
| `mempalace_status` | Palace 概覽 + AAAK 規格 + 記憶協議 |
| `mempalace_list_wings` | 列出 Wing 及其計數 |
| `mempalace_list_rooms` | 列出 Wing 內的 Room |
| `mempalace_get_taxonomy` | 完整 Wing → Room → 計數 樹 |
| `mempalace_search` | 語義搜尋（支援 wing/room 過濾） |
| `mempalace_check_duplicate` | 歸檔前檢查重複 |
| `mempalace_get_aaak_spec` | AAAK 方言參考 |

### Palace 寫入（2 個）
| 工具 | 功能 |
|------|------|
| `mempalace_add_drawer` | 歸檔原文內容 |
| `mempalace_delete_drawer` | 按 ID 刪除 |

### Knowledge Graph（5 個）
| 工具 | 功能 |
|------|------|
| `mempalace_kg_query` | 實體關係查詢（支援時間過濾） |
| `mempalace_kg_add` | 新增事實 |
| `mempalace_kg_invalidate` | 標記事實已結束 |
| `mempalace_kg_timeline` | 實體的時間線敘事 |
| `mempalace_kg_stats` | 圖譜概覽 |

### 導航（3 個）
| 工具 | 功能 |
|------|------|
| `mempalace_traverse` | 從 Room 出發遍歷圖 |
| `mempalace_find_tunnels` | 找到跨 Wing 的連接 |
| `mempalace_graph_stats` | 圖連接性概覽 |

### Agent Diary（2 個）
| 工具 | 功能 |
|------|------|
| `mempalace_diary_write` | 寫 AAAK 日記條目 |
| `mempalace_diary_read` | 讀取最近的日記條目 |
