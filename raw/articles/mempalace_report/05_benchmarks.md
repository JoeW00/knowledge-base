# 5. 基準測試與重現方法

## 測試框架總覽

MemPalace 提供 4 個基準測試執行器，全部在 `benchmarks/` 目錄下：

| 檔案 | 測試對象 | 資料集 |
|------|----------|--------|
| `longmemeval_bench.py` | LongMemEval — 長期記憶評估 | 500 個問答對 |
| `locomo_bench.py` | LoCoMo — 對話記憶理解 | session/dialog 粒度 |
| `convomem_bench.py` | ConvoMem — Salesforce 7.5 萬 QA | 6 個類別 |
| `membench_bench.py` | MemBench — ACL 2025 | 多類別 |

## LongMemEval 基準測試（核心）

### 測試模式

LongMemEval 是最重要的基準測試，支援多種模式：

| 模式 | 說明 | R@5 分數 |
|------|------|----------|
| `raw` | 純 ChromaDB 向量搜尋，零 API | **96.6%** |
| `hybrid_v1` ~ `hybrid_v4` | 向量搜尋 + LLM rerank | 最高 **100%** |
| `palace` | 使用 Wing/Room 結構過濾 | — |
| `diary` | 使用 Agent Diary 模式 | — |

### 重現步驟

```bash
# 1. 下載 LongMemEval 資料集（見 benchmarks/README.md）
# 2. 快速測試（20 題，~30 秒）
python benchmarks/longmemeval_bench.py /path/to/longmemeval_s_cleaned.json --limit 20

# 3. 完整測試（500 題，~5 分鐘）
python benchmarks/longmemeval_bench.py /path/to/longmemeval_s_cleaned.json
```

### Hybrid 模式的 Rerank 機制

- 先用 ChromaDB 向量搜尋取得候選結果
- 再用 LLM（如 Claude Haiku）對候選結果重新排序
- Hybrid + Haiku rerank 達到 100%（500/500）
- **注意**：rerank pipeline 尚未完全公開在 benchmark 腳本中

## LoCoMo 基準測試

- 支援 session-level 和 dialog-level 粒度
- 支援 hybrid scoring 和 palace room assignment
- Raw session level 達到 60.3% R@10

## ConvoMem 基準測試

- Salesforce 7.5 萬個 QA 對
- 6 個對話記憶類別
- 測試大規模對話記憶的能力

## MemBench 基準測試

- ACL 2025 學術基準
- 多類別記憶評估

## 已知問題與誠實聲明

作者在 README 中公開承認了以下問題：

1. **AAAK token 範例有誤**：原本用 `len(text)//3` 估算 token 數，實際用 OpenAI tokenizer 計算後，AAAK 在小規模下不省 token
2. **"30x 無損壓縮" 是誇大**：AAAK 是有損的，在 LongMemEval 上 84.2% vs raw 的 96.6%
3. **"+34% palace boost" 有誤導**：這是標準的 ChromaDB metadata filtering，不是創新機制
4. **"矛盾偵測" 未整合**：`fact_checker.py` 存在但未接入知識圖譜操作
5. **"100% with Haiku rerank" 未公開**：結果檔案存在但 pipeline 未在公開腳本中

## 測試套件

`tests/` 目錄有 11 個測試檔案：

| 測試檔 | 涵蓋範圍 |
|--------|----------|
| `conftest.py` | 共用 fixtures |
| `test_config.py` | 設定載入 |
| `test_convo_miner.py` | 對話挖掘 |
| `test_dialect.py` | AAAK 方言壓縮 |
| `test_knowledge_graph.py` | 知識圖譜（時序三元組、實體、查詢、時間線、WAL 模式） |
| `test_mcp_server.py` | MCP 伺服器（協議層、讀寫搜尋 KG diary 工具） |
| `test_miner.py` | 專案挖掘（gitignore 處理、掃描） |
| `test_normalize.py` | 格式標準化 |
| `test_searcher.py` | 搜尋 API |
| `test_split_mega_files.py` | 大檔案拆分 |
| `test_version_consistency.py` | 版本一致性 |

執行測試：
```bash
pytest tests/ -v
```
