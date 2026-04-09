# 3. 原始碼模組詳解

MemPalace 的核心程式碼全部在 `mempalace/` 目錄下，共 21 個 Python 檔案。以下按功能分類說明每個模組的職責與關鍵實作細節。

---

## 3.1 入口與設定

### `__init__.py`
- 匯出 `main` 函式與 `__version__`
- 是整個套件的進入點

### `__main__.py`
- 讓 `python -m mempalace` 可以執行
- 只有一行：呼叫 `main()`

### `version.py`
- 單行檔案：`__version__ = "3.0.0"`

### `cli.py`（16,490 bytes）
- CLI 入口點，使用 `argparse` 建立子命令
- **命令清單**：
  - `init <dir>` — 引導式設定 + AAAK 啟動
  - `mine <dir>` — 挖掘專案檔案（支援 `--mode convos`、`--extract general`、`--wing`）
  - `split <dir>` — 拆分合併的逐字稿（支援 `--dry-run`、`--min-sessions`）
  - `search "query"` — 語義搜尋（支援 `--wing`、`--room`、`--top`）
  - `compress` — AAAK 壓縮
  - `wake-up` — 載入 L0 + L1 context
  - `repair` — 修復損壞的 palace
  - `status` — 宮殿概覽

### `config.py`（4,443 bytes）
- 設定載入優先順序：環境變數 > config 檔案 > 預設值
- 定義 `DEFAULT_PALACE_PATH = ~/.mempalace/palace`
- 定義 `TOPIC_WINGS` 映射（把主題關鍵字對應到 Wing）
- 定義 `HALL_KEYWORDS` 映射（把關鍵字對應到五種 Hall）

---

## 3.2 資料攝取（Ingest）

### `miner.py`（20,352 bytes）— 專案檔案挖掘
- **.gitignore 感知**：讀取 `.gitignore` 規則，跳過不該索引的檔案
- **檔案掃描**：遞迴掃描目錄，支援多種文字檔格式
- **Room 路由**：根據檔案路徑和內容判斷應歸入哪個 Room
- **文字分塊**：將大檔案切成適合 ChromaDB 的 chunk
- **Palace 歸檔**：寫入 ChromaDB，附帶 wing/hall/room metadata

### `convo_miner.py`（11,970 bytes）— 對話匯入
- **交換對分塊（Exchange-pair chunking）**：以「一問一答」為單位切分對話
- **主題偵測**：從內容推測應歸入哪個 Room
- **支援模式**：
  - `convos` — 純對話匯入
  - `convos --extract general` — 自動分類為 decisions/milestones/problems 等

### `normalize.py`（10,892 bytes）— 格式標準化
- 支援 **5 種對話格式**轉換為統一 transcript：
  1. Claude Code JSONL
  2. Codex CLI JSONL
  3. Claude.ai JSON
  4. ChatGPT JSON
  5. Slack JSON
  6. 純文字
- 輸出統一的 `[role]: content` 格式

### `split_mega_files.py`（10,251 bytes）— 拆分大檔案
- 偵測 session 邊界（如 "Claude Code v" 標記）
- 將合併的逐字稿拆成單獨的 per-session 檔案
- 支援 `--dry-run` 預覽和 `--min-sessions` 過濾

### `general_extractor.py`（14,067 bytes）— 通用萃取器
- 從對話中萃取 5 種記憶類型：
  1. **Decisions** — 已做的決策
  2. **Preferences** — 偏好設定
  3. **Milestones** — 里程碑事件
  4. **Problems** — 遇到的問題
  5. **Emotional context** — 情緒脈絡
- 使用關鍵字和正則表達式進行啟發式分類（不依賴 LLM）

---

## 3.3 搜尋與導航

### `searcher.py`（4,313 bytes）— 語義搜尋
- 透過 ChromaDB 的向量搜尋進行語義查詢
- 支援 wing/room 過濾
- 提供兩種介面：
  - CLI 模式（印出結果）
  - 程式化模式（回傳 dict）

### `palace_graph.py`（7,188 bytes）— 宮殿導航圖
- 從 ChromaDB metadata 建立 Room 節點與 Wing 邊的圖結構
- **BFS 遍歷**：從一個 Room 出發，廣度優先搜尋相關內容
- **Tunnel 尋找**：找到跨 Wing 的同名 Room 連接

### `room_detector_local.py`（9,911 bytes）— 本地 Room 偵測
- 從資料夾結構和檔名模式推測 Room 名稱
- 生成 `mempalace.yaml` 設定檔
- 用於 `mempalace init` 的自動偵測階段

---

## 3.4 知識圖譜

### `knowledge_graph.py`（14,824 bytes）— 時序實體關係圖
- 使用 **SQLite** 儲存（對比 Zep 的 Neo4j）
- 支援時間有效性視窗：
  ```python
  kg.add_triple("Kai", "works_on", "Orion", valid_from="2025-06-01")
  kg.invalidate("Kai", "works_on", "Orion", ended="2026-03-01")
  ```
- 核心操作：
  - `add_triple()` — 新增事實
  - `query_entity()` — 查詢實體（支援 `as_of` 時間點查詢）
  - `invalidate()` — 標記事實已結束
  - `timeline()` — 取得實體的時間線敘事
  - `stats()` — 圖譜統計資訊

### `entity_detector.py`（21,874 bytes）— 實體偵測
- 從文件內容自動偵測人物和專案
- 使用信號模式：person verbs、dialogue markers、project verbs
- 輸出 `{entity_name: entity_type}` 映射

### `entity_registry.py`（23,064 bytes）— 實體註冊表
- 持久化實體登錄，支援 onboarding 種子資料
- Wikipedia 查詢來判斷未知字詞是否為常見名詞
- 上下文消歧義：處理同名人物/專案

---

## 3.5 AAAK 方言與壓縮

### `dialect.py`（33,822 bytes）— AAAK 壓縮方言
- **AAAK** 是一套有損的縮寫系統，設計用於大規模重複實體壓縮
- 核心功能：
  - **實體代碼**：`Kai → KAI`、`Driftwood → DFT`
  - **情緒代碼**：標記對話情緒狀態
  - **旗標系統**：標記重要性等級
  - **Zettel 編碼**：將結構化資訊壓縮
  - **Layer 1 生成**：產生 ~120 token 的關鍵事實摘要
  - **壓縮統計**：計算壓縮比率
- **重要警告**：AAAK 目前在 LongMemEval 上 84.2% R@5，低於 raw mode 的 96.6%

---

## 3.6 MCP 伺服器

### `mcp_server.py`（29,109 bytes）— MCP 協議伺服器
- 暴露 **19 個工具** 給 AI 使用
- **Palace 讀取工具**（7 個）：status、list_wings、list_rooms、get_taxonomy、search、check_duplicate、get_aaak_spec
- **Palace 寫入工具**（2 個）：add_drawer、delete_drawer
- **Knowledge Graph 工具**（5 個）：kg_query、kg_add、kg_invalidate、kg_timeline、kg_stats
- **導航工具**（3 個）：traverse、find_tunnels、graph_stats
- **Agent Diary 工具**（2 個）：diary_write、diary_read
- AI 從 `mempalace_status` 回應自動學習 AAAK 和記憶協議

---

## 3.7 其他工具

### `layers.py`（17,224 bytes）— 四層記憶堆疊
- 實作 L0（身份）、L1（關鍵事實）、L2（按需）、L3（深度搜尋）
- `wake-up` 命令的核心實作

### `onboarding.py`（18,707 bytes）— 引導式設定
- 互動式首次設定流程：
  1. 模式選擇
  2. 人物/專案登錄
  3. Wing 設定生成
  4. AAAK 啟動文件生成

### `spellcheck.py`（10,381 bytes）— 拼字檢查
- 使用 `autocorrect` 套件 + 編輯距離守衛
- 保留技術術語、CamelCase、實體名稱、URL

---

## 3.8 模組依賴關係圖

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
