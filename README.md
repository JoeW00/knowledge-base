# LLM Knowledge Base

> v0.1.0

以 LLM 作為知識維護 agent 的個人知識庫系統。

靈感來自 Andrej Karpathy 的「LLM Knowledge Bases」構想，使用 **Claude Code + Obsidian** 實作，讓 LLM 從「寫程式的工具」轉變為「維護知識的 agent」。

## 核心概念

```
收集素材 → /ingest 自動編譯 → wiki 結構化知識 → /ask 研究問答 → 知識回饋
```

- `raw/` 存放原始素材（唯讀），LLM 絕不修改
- `wiki/` 由 LLM 全權維護，使用者幾乎不手動編輯
- `CLAUDE.md` 和 slash commands 定義系統行為——**prompt 是最有價值的資產**

## 目錄結構

```
~/knowledge-base/
├── raw/                  # 原始素材（文章、論文、逐字稿等）
│   ├── articles/         # Web Clipper 擷取或手動放入的文章
│   ├── papers/           # 學術論文 PDF
│   ├── repos/            # git repo clone
│   ├── datasets/         # 結構化資料集
│   ├── images/           # 圖片素材
│   └── transcripts/      # 逐字稿
├── wiki/                 # LLM 維護的結構化知識
│   ├── index.md          # 自動維護的全域索引
│   ├── concepts/         # 概念文章（300–800 字）
│   ├── summaries/        # 每篇 raw 素材的摘要
│   ├── topics/           # 主題聚合頁
│   └── explorations/     # 有價值的問答回存
├── outputs/              # 查詢產出（報告、簡報、圖表）
├── scripts/              # 自訂工具
├── .claude/commands/     # slash command 定義
└── CLAUDE.md             # 系統操作手冊
```

> 本 repo 只包含系統骨架與工作流定義，不含實際內容。

## 五個工作流

| 指令 | 用途 | 說明 |
|------|------|------|
| `/compile <path>` | 單檔編譯 | 將一篇 raw 素材轉為 summary + concept 文章 |
| `/ingest` | 批次編譯 | 自動偵測未處理的素材，轉換格式並逐一 compile |
| `/index` | 重建索引 | 重新生成 `wiki/index.md`（統計、分類、孤兒警告） |
| `/ask <問題>` | 研究問答 | 從 wiki 中組織答案，可回存至 explorations |
| `/lint` | 健康檢查 | 檢查連結完整性、內容一致性、缺漏與 tag 整理 |

### 工作流串接

```
/ingest
  ├── 盤點 raw/ 中所有檔案
  ├── 轉換非 md 檔（使用 markitdown）
  ├── 比對 wiki/summaries/ 找出未處理檔案
  ├── 逐一呼叫 /compile --batch（跳過逐次 index）
  └── 全部完成後統一執行 /index
```

### 每日自動化

```
/loop 24h /ingest --auto
```

`--auto` 模式下無新素材時自動跳過，有新素材時跳過確認直接執行。

## 快速開始

### 環境需求

- macOS
- [Claude Code](https://claude.ai/code)
- [Obsidian](https://obsidian.md)（選用，作為 wiki 閱讀器）
- Python 3.14+（`uv` 管理）
- [markitdown](https://github.com/microsoft/markitdown) — PDF / docx / pptx 轉 Markdown

### 安裝

```bash
git clone <repo-url> ~/knowledge-base
cd ~/knowledge-base

# 安裝 PDF / 文件轉換工具
uv tool install markitdown
```

### 建立目錄

```bash
mkdir -p raw/{articles,papers,repos,datasets,images,transcripts}
mkdir -p wiki/{concepts,summaries,topics,explorations}
mkdir -p outputs/{reports,slides,charts}
```

### 開始使用

```bash
cd ~/knowledge-base
claude
```

在 Claude Code 中：

```
# 放入素材後，自動偵測並編譯
/ingest

# 問一個研究問題
/ask Transformer 的 attention mechanism 和 KV cache 的關係是什麼？

# 每週健康檢查
/lint
```

## 設計決策

- **Obsidian wiki-link 語法**：文章之間用 `[[concept-name]]` 互連，搭配 Obsidian graph view 視覺化知識網路
- **Summary 的 `source:` 欄位**：frontmatter 中記錄來源路徑，是 `/ingest` 判斷「已處理」的唯一依據
- **PDF 轉換**：所有 PDF / docx / pptx 統一使用 `markitdown` 轉換
- **`--batch` 旗標**：`/compile --batch` 跳過 `/index`，由 `/ingest` 在批次結束後統一執行一次，避免重複 index

## Roadmap

- **Wiki Search MCP Server** — 把 wiki 搜尋包成 local MCP server，掛進 Claude Code，讓 `/ask` 能更精準地檢索知識庫
- **Marp 簡報輸出** — 從 wiki 內容直接產出簡報，用於客戶 demo 或內部分享
- **Manifest 升級** — 從 frontmatter `source:` 反查升級到 `.compiled.json`，加上 file hash 偵測內容變更，避免重複 compile 未修改的檔案
- **whisper.cpp 逐字稿** — 自動處理 podcast 和影片音檔，轉為逐字稿存入 `raw/transcripts/`
- **Synthetic data + finetune**（長期）— 讓 LLM 將 wiki 內容內化到模型權重中，而非僅依賴 context window

## 授權

私人專案，僅供參考。
