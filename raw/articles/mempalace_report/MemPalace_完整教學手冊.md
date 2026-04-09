# MemPalace 完整教學手冊

> **來源倉庫**：https://github.com/milla-jovovich/mempalace
> **版本**：v3.0.0 | **授權**：MIT | **語言**：Python 3.9+
> **整理日期**：2026-04-09

---

## 目錄

1. [專案概述](#1-專案概述)
2. [宮殿架構](#2-宮殿架構palace-architecture)
3. [原始碼模組詳解](#3-原始碼模組詳解)
4. [自動儲存 Hook 機制](#4-自動儲存-hook-機制)
5. [基準測試與重現方法](#5-基準測試與重現方法)
6. [整合方式與使用範例](#6-整合方式與使用範例)
7. [從零實作指南](#7-從零實作指南)
8. [已知問題與作者聲明](#8-已知問題與作者聲明)
9. [CI/CD 與貢獻指南](#9-cicd-與貢獻指南)

---

# 1. 專案概述

## 一句話總結

MemPalace 是一套**完全本地端運行的 AI 記憶系統**，將你與 AI 的所有對話、決策、程式碼討論，以「宮殿記憶法」的層級結構存入 ChromaDB 向量資料庫，達到目前公開基準測試中最高的 **96.6% 召回率**（LongMemEval R@5），且**不需要任何 API 金鑰、不上傳任何資料到雲端**。

## 核心理念

| 原則 | 說明 |
|------|------|
| **原文逐字儲存（Verbatim First）** | 不做摘要、不做萃取，保留你的原始對話。96.6% 的成績來自這個「raw mode」 |
| **本地優先（Local First）** | 所有資料留在你的電腦，ChromaDB + SQLite，零雲端依賴 |
| **零 API（Zero API by default）** | 核心功能不需要任何 API key，安裝完就能用 |
| **結構即產品** | Wing → Room → Closet → Drawer 的階層結構帶來 +34% 的檢索提升 |

## 問題場景

你每天與 Claude、ChatGPT、Copilot 對話，累積了大量決策脈絡。**六個月的每日 AI 使用 ≈ 1950 萬 tokens**，這些對話在 session 結束後就消失了。

| 方案 | 載入 Token 數 | 年成本 |
|------|-------------|--------|
| 全部貼上 | 1950 萬（超出任何 context window） | 不可能 |
| LLM 摘要 | ~65 萬 | ~$507/年 |
| **MemPalace wake-up** | **~170 tokens** | **~$0.70/年** |
| **MemPalace + 5 次搜尋** | **~13,500 tokens** | **~$10/年** |

## 基準測試成績摘要

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

## 技術規格

- **版本**: v3.0.0
- **依賴**: `chromadb>=0.5.0,<0.7`, `pyyaml>=6.0`
- **建置系統**: Hatchling
- **程式碼風格**: Ruff, 100 字元行寬

---

# 2. 宮殿架構（Palace Architecture）

## 記憶宮殿的隱喻

MemPalace 借用古希臘演說家的「記憶宮殿」技巧：把想法放在一棟建築物的不同房間中，走過建築就能找到想法。

```
  ┌─────────────────────────────────────────────────────────────┐
  │  WING: Person (人物翼)                                      │
  │                                                            │
  │    ┌──────────┐  ──hall──  ┌──────────┐                    │
  │    │  Room A  │            │  Room B  │                    │
  │    └────┬─────┘            └──────────┘                    │
  │         │                                                  │
  │         ▼                                                  │
  │    ┌──────────┐      ┌──────────┐                          │
  │    │  Closet  │ ───▶ │  Drawer  │                          │
  │    └──────────┘      └──────────┘                          │
  └─────────┼──────────────────────────────────────────────────┘
            │
          tunnel (隧道)
            │
  ┌─────────┼──────────────────────────────────────────────────┐
  │  WING: Project (專案翼)                                     │
  │         │                                                  │
  │    ┌────┴─────┐  ──hall──  ┌──────────┐                    │
  │    │  Room A  │            │  Room C  │                    │
  │    └────┬─────┘            └──────────┘                    │
  │         │                                                  │
  │         ▼                                                  │
  │    ┌──────────┐      ┌──────────┐                          │
  │    │  Closet  │ ───▶ │  Drawer  │                          │
  │    └──────────┘      └──────────┘                          │
  └─────────────────────────────────────────────────────────────┘
```

## 五大結構元素

| 元素 | 定義 | 範例 |
|------|------|------|
| **Wing（翼）** | 一個人物或一個專案的獨立命名空間 | `wing_kai`, `wing_driftwood` |
| **Room（房間）** | Wing 內的具體主題 | `auth-migration`, `ci-pipeline` |
| **Hall（走廊）** | 同 Wing 內連接相關 Room 的通道，代表記憶類型 | `hall_facts`, `hall_events` |
| **Tunnel（隧道）** | 跨 Wing 的同名 Room 自動連接 | auth-migration 出現在三個 Wing 中 |
| **Closet & Drawer** | Closet = 摘要指標；Drawer = 原始逐字檔案 | Closet 指向 Drawer |

### 五種標準 Hall

| Hall | 記憶類型 |
|------|----------|
| `hall_facts` | 已做的決定、鎖定的選擇 |
| `hall_events` | 工作階段、里程碑、除錯記錄 |
| `hall_discoveries` | 突破性發現、新見解 |
| `hall_preferences` | 習慣、喜好、意見 |
| `hall_advice` | 建議與解決方案 |

### Tunnel 跨域連接範例

```
wing_kai       / hall_events / auth-migration  → "Kai 除錯了 OAuth token refresh"
wing_driftwood / hall_facts  / auth-migration  → "團隊決定把 auth 遷移到 Clerk"
wing_priya     / hall_advice / auth-migration  → "Priya 核准 Clerk 而非 Auth0"
```

## 結構對檢索的影響（+34%）

| 搜尋範圍 | R@10 | 提升幅度 |
|----------|------|----------|
| 搜尋所有 Closet | 60.9% | 基準 |
| 搜尋特定 Wing | 73.1% | +12% |
| Wing + Hall | 84.8% | +24% |
| **Wing + Room** | **94.8%** | **+34%** |

## 四層記憶堆疊（Memory Stack）

| 層級 | 內容 | 大小 | 何時載入 |
|------|------|------|----------|
| **L0** | 身份 — 這個 AI 是誰？ | ~50 tokens | 永遠載入 |
| **L1** | 關鍵事實 — 團隊、專案、偏好 | ~120 tokens (AAAK) | 永遠載入 |
| **L2** | Room 回憶 — 近期 session、當前專案 | 按需 | 主題出現時 |
| **L3** | 深度搜尋 — 跨所有 Closet 的語義查詢 | 按需 | 明確要求時 |

AI 醒來時只載入 L0 + L1（~170 tokens）就知道你的世界。搜尋只在需要時觸發。

## AAAK 壓縮方言（實驗性）

AAAK 是一套有損的縮寫系統，設計用於大規模重複實體壓縮。**任何能讀文字的 LLM 都能理解它**，不需要 decoder。

**目前狀態**：
- AAAK 是有損的，不是無損壓縮
- 小規模下不省 token
- 大規模重複實體時可以省 token
- **LongMemEval 上 84.2% R@5 vs raw 的 96.6%** — raw mode 更好
- MemPalace 的儲存預設是 raw verbatim text

## 設定檔

### `~/.mempalace/config.json`（全域設定）
```json
{
  "palace_path": "/custom/path/to/palace",
  "collection_name": "mempalace_drawers",
  "people_map": {"Kai": "KAI", "Priya": "PRI"}
}
```

### `~/.mempalace/wing_config.json`（Wing 設定）
```json
{
  "default_wing": "wing_general",
  "wings": {
    "wing_kai": {"type": "person", "keywords": ["kai", "kai's"]},
    "wing_driftwood": {"type": "project", "keywords": ["driftwood", "analytics", "saas"]}
  }
}
```

### `~/.mempalace/identity.txt`（身份檔）
純文字，成為 Layer 0，每次 session 都載入。

---

# 3. 原始碼模組詳解

MemPalace 核心在 `mempalace/` 目錄，共 21 個 Python 檔案。

## 模組總覽

| 模組 | 大小 | 功能 |
|------|------|------|
| `cli.py` | 16 KB | CLI 入口，8 個子命令 |
| `config.py` | 4 KB | 設定載入（環境變數 > config > 預設值） |
| `miner.py` | 20 KB | 專案檔案挖掘（gitignore 感知、分塊、歸檔） |
| `convo_miner.py` | 12 KB | 對話匯入（exchange-pair chunking） |
| `normalize.py` | 11 KB | 5+ 種對話格式標準化 |
| `split_mega_files.py` | 10 KB | 大檔案拆分 |
| `general_extractor.py` | 14 KB | 5 種記憶類型啟發式萃取 |
| `searcher.py` | 4 KB | ChromaDB 語義搜尋 |
| `palace_graph.py` | 7 KB | Room/Wing 圖 + BFS 遍歷 + Tunnel |
| `room_detector_local.py` | 10 KB | 從資料夾結構推測 Room |
| `knowledge_graph.py` | 15 KB | SQLite 時序實體關係圖 |
| `entity_detector.py` | 22 KB | 自動偵測人物/專案 |
| `entity_registry.py` | 23 KB | 持久化實體登錄 + 消歧義 |
| `dialect.py` | 34 KB | AAAK 壓縮方言 |
| `layers.py` | 17 KB | 四層記憶堆疊 |
| `mcp_server.py` | 29 KB | MCP 伺服器（19 個工具） |
| `onboarding.py` | 19 KB | 互動式首次設定 |
| `spellcheck.py` | 10 KB | 拼字檢查（保留技術術語） |
| `version.py` | 87 B | 版本號 |
| `__init__.py` | 155 B | 套件入口 |
| `__main__.py` | 75 B | `python -m mempalace` 入口 |

## 資料攝取流程

### 專案檔案挖掘（`miner.py`）
1. 讀取 `.gitignore` 規則
2. 遞迴掃描目錄中的文字檔
3. 每個檔案切成適合 ChromaDB embedding 的 chunk
4. 根據檔案路徑/內容推測 wing 和 room
5. 寫入 ChromaDB，metadata 含 `{wing, hall, room, source_file, timestamp}`

### 對話匯入（`convo_miner.py` + `normalize.py`）
1. 偵測格式（Claude Code JSONL / ChatGPT JSON / Slack JSON / 純文字等）
2. 轉換為統一的 `[role]: content` transcript
3. 以「一問一答」為單位分塊
4. 關鍵字匹配推測 wing/hall/room
5. 選用 `general_extractor.py` 進一步分類為 decisions/milestones/problems/preferences/emotional

### 支援的對話格式
1. Claude Code JSONL
2. Codex CLI JSONL
3. Claude.ai JSON
4. ChatGPT JSON
5. Slack JSON
6. 純文字

## 搜尋與導航

- **`searcher.py`**：ChromaDB 向量搜尋，支援 wing/room 過濾
- **`palace_graph.py`**：BFS 遍歷 Room 圖，找到跨 Wing 的 Tunnel 連接
- **`room_detector_local.py`**：從資料夾結構和檔名模式推測 Room

## 知識圖譜（`knowledge_graph.py`）

使用 SQLite 儲存時序實體關係三元組：

```python
from mempalace.knowledge_graph import KnowledgeGraph
kg = KnowledgeGraph()
kg.add_triple("Kai", "works_on", "Orion", valid_from="2025-06-01")
kg.query_entity("Kai")              # 查詢當前關係
kg.query_entity("Maya", as_of="2026-01-20")  # 時間點查詢
kg.timeline("Orion")                # 專案時間線
kg.invalidate("Kai", "works_on", "Orion", ended="2026-03-01")  # 標記結束
```

## MCP Server（`mcp_server.py`）— 19 個工具

| 類別 | 工具數 | 工具名稱 |
|------|--------|----------|
| Palace 讀取 | 7 | status, list_wings, list_rooms, get_taxonomy, search, check_duplicate, get_aaak_spec |
| Palace 寫入 | 2 | add_drawer, delete_drawer |
| Knowledge Graph | 5 | kg_query, kg_add, kg_invalidate, kg_timeline, kg_stats |
| 導航 | 3 | traverse, find_tunnels, graph_stats |
| Agent Diary | 2 | diary_write, diary_read |

## 模組依賴關係

```
cli.py ─────┬──→ miner.py ──→ searcher.py (ChromaDB)
            ├──→ convo_miner.py ──→ normalize.py
            │                   ──→ general_extractor.py
            ├──→ onboarding.py ──→ entity_detector.py
            │                  ──→ entity_registry.py
            │                  ──→ room_detector_local.py
            ├──→ layers.py ──→ dialect.py
            ├──→ searcher.py
            └──→ split_mega_files.py

mcp_server.py ──→ searcher.py
              ──→ knowledge_graph.py
              ──→ palace_graph.py
              ──→ dialect.py
              ──→ layers.py
```

---

# 4. 自動儲存 Hook 機制

## 兩種 Hook

| Hook | 觸發時機 | 行為 |
|------|----------|------|
| **Save Hook** (`mempal_save_hook.sh`) | 每 15 條人類訊息 | 阻斷 AI，要求存入 Palace |
| **PreCompact Hook** (`mempal_precompact_hook.sh`) | Context 壓縮前 | 緊急儲存所有內容 |

## Save Hook 流程

```
User 訊息 → AI 回應 → Stop hook 觸發
                            ↓
                    計算人類訊息數
                            ↓
              ┌─── < 15 ──→ echo "{}" (放行)
              └─── ≥ 15 ──→ block → AI 存記憶 → 再次 Stop → flag=true → 放行
```

**防無限迴圈**：`stop_hook_active` 旗標確保只 block 一次。

## PreCompact Hook 流程

永遠 block → AI 儲存所有內容 → 壓縮正常進行。

## 安裝

### Claude Code（`.claude/settings.local.json`）
```json
{
  "hooks": {
    "Stop": [{
      "matcher": "*",
      "hooks": [{"type": "command", "command": "/path/to/hooks/mempal_save_hook.sh", "timeout": 30}]
    }],
    "PreCompact": [{
      "hooks": [{"type": "command", "command": "/path/to/hooks/mempal_precompact_hook.sh", "timeout": 30}]
    }]
  }
}
```

### Gemini CLI（`~/.gemini/settings.json`）
```json
{
  "hooks": {
    "PreCompress": [{
      "matcher": "*",
      "hooks": [{"type": "command", "command": "/path/to/hooks/mempal_precompact_hook.sh"}]
    }]
  }
}
```

## 設定參數

| 參數 | 預設值 | 說明 |
|------|--------|------|
| `SAVE_INTERVAL` | 15 | 每幾條訊息觸發儲存 |
| `STATE_DIR` | `~/.mempalace/hook_state/` | Hook 狀態檔位置 |
| `MEMPAL_DIR` | 空白 | 設定後自動執行 `mempalace mine` |

## 除錯

```bash
cat ~/.mempalace/hook_state/hook.log
```

## 安全措施

- Session ID 用 `tr -cd 'a-zA-Z0-9_-'` 清理，防路徑遍歷
- 跳過 `<command-message>` 系統訊息，只計算真人輸入

---

# 5. 基準測試與重現方法

## 四個基準測試執行器

| 檔案 | 測試對象 | 資料集 |
|------|----------|--------|
| `longmemeval_bench.py` | LongMemEval | 500 個問答對 |
| `locomo_bench.py` | LoCoMo | session/dialog 粒度 |
| `convomem_bench.py` | ConvoMem (Salesforce) | 7.5 萬 QA |
| `membench_bench.py` | MemBench (ACL 2025) | 多類別 |

## LongMemEval 重現

```bash
# 快速測試（20 題，~30 秒）
python benchmarks/longmemeval_bench.py /path/to/longmemeval_s_cleaned.json --limit 20

# 完整測試（500 題，~5 分鐘）
python benchmarks/longmemeval_bench.py /path/to/longmemeval_s_cleaned.json
```

支援模式：`raw`、`hybrid_v1`~`hybrid_v4`、`palace`、`diary`

## 測試套件

```bash
pytest tests/ -v
```

11 個測試檔案涵蓋：config、convo_miner、dialect、knowledge_graph、mcp_server、miner、normalize、searcher、split_mega_files、version_consistency。

---

# 6. 整合方式與使用範例

## 整合方式

| 工具 | 整合方式 | 自動化程度 |
|------|----------|-----------|
| Claude Code | MCP Server + Hooks | 全自動 |
| Gemini CLI | MCP Server + Hooks | 全自動 |
| ChatGPT / Cursor | MCP Server | 半自動 |
| 本地模型 | CLI / Python API | 手動 |

## Claude Code 整合

```bash
# 1. 註冊 MCP Server
claude mcp add mempalace -- python -m mempalace.mcp_server

# 2. 設定 Hook（見第 4 章）
# 完成！AI 自動搜尋記憶、自動儲存
```

## Gemini CLI 整合

```bash
git clone https://github.com/milla-jovovich/mempalace.git && cd mempalace
python3 -m venv .venv && .venv/bin/pip install -e .
.venv/bin/python3 -m mempalace init .
gemini mcp add mempalace /path/.venv/bin/python3 -m mempalace.mcp_server --scope user
```

## 本地模型整合

```bash
# 方法 1：Wake-up
mempalace wake-up > context.txt
# 貼入 system prompt

# 方法 2：CLI 搜尋
mempalace search "auth decisions" > results.txt
# 放入 prompt

# 方法 3：Python API
from mempalace.searcher import search_memories
results = search_memories("auth decisions", palace_path="~/.mempalace/palace")
```

## 實際使用情境

### Solo 開發者

```bash
mempalace mine ~/chats/orion/  --mode convos --wing orion
mempalace mine ~/chats/nova/   --mode convos --wing nova
mempalace search "database decision" --wing orion
# → "選了 Postgres 而非 SQLite，因為 Orion 需要並發寫入"
```

### 團隊主管

```bash
mempalace mine ~/exports/slack/ --mode convos --wing driftwood
mempalace search "Soren sprint" --wing driftwood
# → 14 個 closet：OAuth 重構、dark mode、component library 遷移
```

## Specialist Agent

```bash
mkdir -p ~/.mempalace/agents
# 建立 reviewer.json、architect.json、ops.json
```

每個 Agent 有專注領域、自己的日記（AAAK 格式），跨 session 持久化。

---

# 7. 從零實作指南

## 快速開始

```bash
pip install mempalace
mempalace init ~/projects/myapp
mempalace mine ~/projects/myapp
mempalace mine ~/chats/ --mode convos
mempalace search "why did we switch to GraphQL"
mempalace status
```

## 完整實作步驟

### 階段 1：環境準備

```bash
# 從原始碼安裝
git clone https://github.com/milla-jovovich/mempalace.git
cd mempalace
pip install -e ".[dev]"
```

### 階段 2：初始化 Palace

```bash
mempalace init ~/projects/myapp
# 互動式設定：身份、人物/專案、Wing、Room 偵測
```

或手動建立：
```bash
mkdir -p ~/.mempalace
echo "我是全端工程師" > ~/.mempalace/identity.txt
# 建立 wing_config.json 和 config.json
```

### 階段 3：挖掘資料

```bash
mempalace mine ~/projects/myapp                              # 專案檔案
mempalace mine ~/chats/ --mode convos --wing myapp           # 對話
mempalace mine ~/chats/ --mode convos --extract general      # 對話 + 自動分類
mempalace split ~/chats/ --dry-run                           # 預覽拆分大檔案
```

### 階段 4：搜尋

```bash
mempalace search "query"                          # 全域搜尋
mempalace search "query" --wing myapp             # Wing 內搜尋
mempalace search "query" --room auth-migration    # Room 內搜尋
mempalace wake-up                                 # 載入 L0+L1 context
```

### 階段 5：MCP Server

```bash
claude mcp add mempalace -- python -m mempalace.mcp_server
```

### 階段 6：Hook

```bash
chmod +x hooks/*.sh
# 在 settings.local.json 中設定（見第 4 章）
```

### 階段 7：Knowledge Graph

```python
from mempalace.knowledge_graph import KnowledgeGraph
kg = KnowledgeGraph()
kg.add_triple("Kai", "works_on", "Orion", valid_from="2025-06-01")
kg.query_entity("Kai")
kg.timeline("Orion")
```

### 階段 8：驗證

```bash
pytest tests/ -v
mempalace status
python benchmarks/longmemeval_bench.py /path/to/data.json --limit 20
```

## 如果要自己從頭寫——建議順序

| Phase | 模組 | 說明 |
|-------|------|------|
| **1. 基礎** | config → searcher → miner → cli | 最小可行版本：init + mine + search |
| **2. 對話** | normalize → convo_miner → split_mega_files | 支援 2-3 種對話格式 |
| **3. 結構** | room_detector → entity_detector → entity_registry → palace_graph | Wing/Room 圖結構 |
| **4. 進階** | knowledge_graph → layers → dialect → general_extractor | 知識圖譜 + 記憶堆疊 + AAAK |
| **5. 整合** | mcp_server → onboarding → hooks | MCP 伺服器 + 自動化 |

## 關鍵技術決策

| 決策 | 選擇 | 原因 |
|------|------|------|
| 向量資料庫 | ChromaDB | 本地、免費、Python 原生 |
| 知識圖譜 | SQLite | 本地、零依賴（vs Neo4j） |
| 壓縮方言 | AAAK（有損） | 任何 LLM 都能讀 |
| 分塊策略 | Exchange-pair | 保留問答脈絡完整性 |
| Metadata | wing/hall/room | +34% 檢索提升 |
| 整合協議 | MCP（19 工具） | AI 自動發現使用 |

---

# 8. 已知問題與作者聲明

作者 Milla Jovovich & Ben Sigman 在 2026-04-07 公開承認以下問題：

### 已修正或修正中的問題

| 問題 | 說明 | 狀態 |
|------|------|------|
| AAAK token 範例有誤 | 用 `len(text)//3` 估算而非真實 tokenizer | 重寫中 |
| "30x 無損壓縮" 誇大 | AAAK 是有損的，84.2% vs 96.6% | 已更正措辭 |
| "+34% palace boost" 誤導 | 是標準 ChromaDB metadata filtering | 已澄清 |
| 矛盾偵測未整合 | `fact_checker.py` 存在但未接入 KG | 修正中（Issue #27） |
| "100% with Haiku rerank" 未公開 | 結果存在但 pipeline 未公開 | 新增中 |
| ChromaDB 版本未固定 | — | Issue #100 |
| Shell injection in hooks | — | Issue #110 |
| macOS ARM64 segfault | — | Issue #74 |

### 仍然成立且可重現的

- **96.6% R@5 on LongMemEval**（raw mode, 500 題, 零 API）— 已被獨立重現
- 完全本地、免費、零雲端
- Palace 架構（wings, rooms, closets, drawers）真實且有用

---

# 9. CI/CD 與貢獻指南

## GitHub Actions CI

```yaml
# .github/workflows/ci.yml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        python-version: ["3.9", "3.11", "3.13"]
    steps:
      - pip install -e ".[dev]"
      - python -m pytest tests/ -v

  lint:
    steps:
      - ruff check .
      - ruff format --check .
```

## 貢獻流程

1. Fork → `git checkout -b feat/my-thing`
2. 寫程式碼 + 測試
3. `pytest tests/ -v` 全部通過
4. Conventional commits: `feat:`, `fix:`, `docs:`, `bench:`
5. 開 PR against `main`

## 程式碼風格

- Ruff, 100 字元行寬
- `snake_case` 函式/變數, `PascalCase` 類別
- 所有模組和 public function 都要有 docstring
- **最小依賴原則**：只有 ChromaDB + PyYAML

## 架構原則

- **Verbatim first** — 永遠不摘要使用者內容
- **Local first** — 零雲端依賴
- **Zero API by default** — 核心功能不需 API key
- **Palace structure matters** — Wing/Hall/Room 不是裝飾

---

# 附錄：CLI 完整命令一覽

```bash
# 設定
mempalace init <dir>                              # 引導式 onboarding

# 挖掘
mempalace mine <dir>                              # 專案檔案
mempalace mine <dir> --mode convos                # 對話
mempalace mine <dir> --mode convos --wing myapp   # 指定 Wing
mempalace mine <dir> --mode convos --extract general  # 自動分類

# 拆分
mempalace split <dir>                             # 拆分大檔案
mempalace split <dir> --dry-run                   # 預覽

# 搜尋
mempalace search "query"                          # 全域搜尋
mempalace search "query" --wing myapp             # Wing 內搜尋
mempalace search "query" --room auth-migration    # Room 內搜尋

# 記憶堆疊
mempalace wake-up                                 # 載入 L0 + L1
mempalace wake-up --wing driftwood                # 專案專用

# 壓縮
mempalace compress --wing myapp                   # AAAK 壓縮

# 狀態
mempalace status                                  # Palace 概覽

# 修復
mempalace repair                                  # 修復損壞的 palace
```

所有命令支援 `--palace <path>` 覆蓋預設路徑。

---

> **本手冊由 6 個平行 sub-agent 同步讀取 GitHub 倉庫所有檔案後整合而成。**
> **個別章節的 Markdown 檔案存放於 `mempalace_report/` 目錄下。**
