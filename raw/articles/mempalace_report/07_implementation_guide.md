# 7. 從零實作指南 — 如何重新建置 MemPalace

本章提供完整的步驟，讓你從零開始重現 MemPalace 系統的核心功能。

---

## 階段 1：環境準備

### 安裝

```bash
# 方法 A：直接安裝（pip）
pip install mempalace

# 方法 B：從原始碼安裝（推薦用於開發）
git clone https://github.com/milla-jovovich/mempalace.git
cd mempalace
pip install -e ".[dev]"    # 含 pytest, ruff 開發依賴
```

### 依賴確認

```bash
python -c "import chromadb; print(chromadb.__version__)"  # 需要 >=0.5.0,<0.7
python -c "import yaml; print(yaml.__version__)"          # 需要 >=6.0
```

---

## 階段 2：初始化 Palace

```bash
mempalace init ~/projects/myapp
```

這個命令會：
1. 啟動互動式 onboarding（`onboarding.py`）
2. 詢問你的身份資訊 → 寫入 `~/.mempalace/identity.txt`（L0）
3. 偵測你的人物和專案 → 生成 `~/.mempalace/wing_config.json`
4. 掃描資料夾結構 → 用 `room_detector_local.py` 推測 Room
5. 生成 AAAK bootstrap 文件（L1 關鍵事實）
6. 建立 ChromaDB 資料庫（預設在 `~/.mempalace/palace`）

### 手動設定（選用）

如果你想完全控制設定：

```bash
# 建立目錄
mkdir -p ~/.mempalace

# 建立身份檔
echo "我是一個全端工程師，專注於 SaaS 產品開發" > ~/.mempalace/identity.txt

# 建立 Wing 設定
cat > ~/.mempalace/wing_config.json << 'EOF'
{
  "default_wing": "wing_general",
  "wings": {
    "wing_kai": {"type": "person", "keywords": ["kai", "kai's"]},
    "wing_myapp": {"type": "project", "keywords": ["myapp", "dashboard", "api"]}
  }
}
EOF

# 建立全域設定
cat > ~/.mempalace/config.json << 'EOF'
{
  "palace_path": "~/.mempalace/palace",
  "collection_name": "mempalace_drawers",
  "people_map": {"Kai": "KAI", "Priya": "PRI"}
}
EOF
```

---

## 階段 3：挖掘資料

### 3a. 挖掘專案檔案

```bash
mempalace mine ~/projects/myapp
```

**底層流程**（`miner.py`）：
1. 讀取 `.gitignore` 規則
2. 遞迴掃描目錄中的文字檔案
3. 每個檔案切成 chunk（適合 ChromaDB embedding）
4. 根據檔案路徑/內容推測 wing 和 room
5. 寫入 ChromaDB，metadata 含 `{wing, hall, room, source_file, timestamp}`

### 3b. 挖掘對話

```bash
# 基本對話匯入
mempalace mine ~/chats/ --mode convos

# 指定 Wing
mempalace mine ~/chats/ --mode convos --wing myapp

# 使用通用萃取器（自動分類為 decisions/milestones/problems 等）
mempalace mine ~/chats/ --mode convos --extract general
```

**底層流程**（`convo_miner.py`）：
1. 偵測檔案格式（Claude Code JSONL / ChatGPT JSON / Slack JSON 等）
2. 使用 `normalize.py` 轉換為統一格式
3. 以「一問一答」為單位切分（exchange-pair chunking）
4. 用關鍵字匹配推測 wing/hall/room
5. 若指定 `--extract general`，用 `general_extractor.py` 進一步分類
6. 寫入 ChromaDB

### 3c. 預先拆分大檔案

```bash
mempalace split ~/chats/ --dry-run      # 先預覽
mempalace split ~/chats/                 # 實際拆分
```

---

## 階段 4：搜尋與使用

### CLI 搜尋

```bash
mempalace search "why did we switch to GraphQL"
mempalace search "auth decision" --wing myapp
mempalace search "database choice" --room database --top 5
```

### Python API 搜尋

```python
from mempalace.searcher import search_memories

results = search_memories(
    query="auth decisions",
    palace_path="~/.mempalace/palace",
    wing="myapp",       # 選填
    room="auth",         # 選填
    top_k=5              # 選填
)

for r in results:
    print(r["content"])
    print(r["metadata"])
```

### Wake-up（載入上下文）

```bash
mempalace wake-up                        # 載入 L0 + L1（~170 tokens）
mempalace wake-up --wing myapp           # 專案專用 context
```

---

## 階段 5：設定 MCP Server

### Claude Code

```bash
claude mcp add mempalace -- python -m mempalace.mcp_server
```

### Gemini CLI

```bash
gemini mcp add mempalace /path/to/.venv/bin/python3 -m mempalace.mcp_server --scope user
```

### 手動啟動

```bash
python -m mempalace.mcp_server
```

完成後 AI 自動獲得 19 個工具，從 `mempalace_status` 回應中學習記憶協議。

---

## 階段 6：設定自動儲存 Hook

```bash
# 確保 hook 可執行
chmod +x hooks/mempal_save_hook.sh hooks/mempal_precompact_hook.sh
```

在 `.claude/settings.local.json`（或對應 AI 工具的設定檔）中加入 hook 設定（見第 4 章）。

---

## 階段 7：設定 Knowledge Graph

```python
from mempalace.knowledge_graph import KnowledgeGraph

kg = KnowledgeGraph()

# 新增事實
kg.add_triple("Kai", "works_on", "Orion", valid_from="2025-06-01")
kg.add_triple("Maya", "assigned_to", "auth-migration", valid_from="2026-01-15")

# 查詢
kg.query_entity("Kai")
# → [Kai → works_on → Orion (current)]

# 時間點查詢
kg.query_entity("Maya", as_of="2026-01-20")
# → [Maya → assigned_to → auth-migration (active)]

# 時間線
kg.timeline("Orion")
# → 專案的時間順序故事

# 標記事實結束
kg.invalidate("Kai", "works_on", "Orion", ended="2026-03-01")
```

---

## 階段 8：設定 Specialist Agent

```bash
mkdir -p ~/.mempalace/agents
```

建立 Agent JSON：

```json
// ~/.mempalace/agents/reviewer.json
{
  "name": "reviewer",
  "focus": "code quality, patterns, bugs",
  "wing": "wing_reviewer"
}
```

在 CLAUDE.md 加入：
```
You have MemPalace agents. Run mempalace_list_agents to see them.
```

---

## 階段 9：驗證與測試

```bash
# 執行測試套件
pytest tests/ -v

# 檢查 Palace 狀態
mempalace status

# 快速基準測試
python benchmarks/longmemeval_bench.py /path/to/data.json --limit 20
```

---

## 如果要從頭自己寫，核心模組實作順序

如果你想**自己重新實作** MemPalace（而非使用現有程式碼），建議按以下順序：

### Phase 1：基礎儲存（最小可行版本）
1. **`config.py`** — 設定載入（路徑、collection name、預設值）
2. **`searcher.py`** — ChromaDB 向量搜尋封裝
3. **`miner.py`** — 檔案掃描 + 分塊 + 寫入 ChromaDB
4. **`cli.py`** — `init`、`mine`、`search` 三個基本命令

### Phase 2：對話支援
5. **`normalize.py`** — 支援至少 2-3 種對話格式
6. **`convo_miner.py`** — exchange-pair chunking
7. **`split_mega_files.py`** — 大檔案拆分

### Phase 3：宮殿結構
8. **`room_detector_local.py`** — 從資料夾結構推測 Room
9. **`entity_detector.py`** — 自動偵測人物/專案
10. **`entity_registry.py`** — 持久化實體登錄
11. **`palace_graph.py`** — Room/Wing 圖結構 + BFS 遍歷

### Phase 4：進階功能
12. **`knowledge_graph.py`** — SQLite 時序三元組
13. **`layers.py`** — 四層記憶堆疊
14. **`dialect.py`** — AAAK 壓縮方言
15. **`general_extractor.py`** — 5 種記憶類型萃取

### Phase 5：整合
16. **`mcp_server.py`** — MCP 協議伺服器（19 個工具）
17. **`onboarding.py`** — 互動式設定流程
18. **Hooks** — 自動儲存 bash 腳本

---

## 關鍵技術決策

| 決策 | 選擇 | 原因 |
|------|------|------|
| 向量資料庫 | ChromaDB | 本地端、免費、Python 原生 |
| 知識圖譜 | SQLite | 本地端、零依賴（vs Neo4j） |
| 壓縮方言 | AAAK（有損） | 任何 LLM 都能讀、不需 decoder |
| Chat 格式 | 統一 transcript | 用 normalize.py 轉換 5+ 格式 |
| 分塊策略 | Exchange-pair | 保留問答脈絡完整性 |
| Metadata | wing/hall/room | 結構化過濾帶來 +34% 檢索提升 |
| MCP | 19 工具 | AI 自動發現並使用 |
| Hook | bash 腳本 | 零 token 成本、本地執行 |
