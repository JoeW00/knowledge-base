# 個人知識庫系統建置復盤

> 基於 Andrej Karpathy 的「LLM Knowledge Bases」構想，在 Mac + Claude Code + Obsidian 環境實作個人知識庫系統的完整過程與決策紀錄。

---

## 一、核心理念

這個系統的本質是：**把 LLM 從「寫程式的工具」轉成「維護知識的 agent」**，而 wiki 本身就是 agent 的長期記憶。

重要原則：
- `raw/` 只進不出，保留原始素材
- `wiki/` 幾乎不手動編輯，全由 LLM 維護
- `CLAUDE.md` 和 slash commands 是系統的靈魂，**prompt 才是真正值錢的資產**
- 成長飛輪：ingest → compile → ask → file back → 下次 ask 的 context 更豐富

## 二、系統架構

```
~/knowledge-base/
├── raw/                  # 原始素材（唯讀）
│   ├── articles/         # 文章與技術指南
│   ├── papers/           # 學術論文（PDF + 轉換後 md）
│   ├── repos/            # repo clone（不進 git）
│   ├── datasets/         # 資料集
│   ├── images/           # 圖片素材
│   └── transcripts/      # 逐字稿
├── wiki/                 # LLM 維護的結構化知識
│   ├── index.md          # 自動維護的總索引
│   ├── concepts/         # 概念文章
│   ├── summaries/        # raw 對應的摘要
│   ├── topics/           # 主題聚合
│   └── explorations/     # Q&A 回存的思考軌跡
├── outputs/              # 查詢產出（不進 git）
│   ├── reports/
│   ├── slides/
│   └── charts/
├── scripts/              # 自訂 CLI / MCP 工具
├── .claude/
│   └── commands/         # slash command 定義
│       ├── compile.md
│       ├── ingest.md
│       ├── index.md
│       ├── ask.md
│       └── lint.md
└── CLAUDE.md             # 系統操作手冊
```

### 各資料夾用途說明

**`raw/`** — 原始素材倉庫，唯讀，Claude Code 絕不修改或刪除其中的檔案。

| 子資料夾 | 用途 |
|----------|------|
| `articles/` | 文章與技術指南。Obsidian Web Clipper 擷取的網頁內容直接存入此處，也可手動放入整理好的 Markdown 檔案 |
| `papers/` | 學術論文。以 PDF 原檔為主，`/ingest` 會自動用 `marker` 或 `markitdown` 轉成同名 `.pdf.md` |
| `repos/` | clone 下來的 git repo，體積大且變動頻繁，`.gitignore` 已排除不進版控 |
| `datasets/` | 結構化資料集（CSV、JSON 等），供分析或訓練用 |
| `images/` | 圖片素材，供 wiki 文章引用 |
| `transcripts/` | Podcast、影片、會議的逐字稿，未來可用 whisper.cpp 自動產生 |

**`wiki/`** — 結構化知識庫，由 LLM 全權維護，使用者幾乎不手動編輯。

| 子資料夾 | 用途 |
|----------|------|
| `concepts/` | 概念文章，每篇 300–800 字解釋一個獨立知識點，文章之間用 `[[wiki-link]]` 互連，結尾必有 `## Related` 區塊 |
| `summaries/` | 每篇 raw 素材對應一份摘要，frontmatter 的 `source:` 欄位指回原始檔，是 `/ingest` 判斷「已處理」的依據 |
| `explorations/` | `/ask` 產出的研究型問答中，使用者認為有保留價值的內容會 file 回此處 |
| `topics/` | 主題聚合頁，把多個相關 concept 串成一個主題敘事 |
| `index.md` | 全域索引，由 `/index` 自動重建，包含統計、分類、最近新增、孤兒文章警告 |

**`outputs/`** — 查詢與分析的產出物，屬於消耗性結果，`.gitignore` 已排除不進版控。

| 子資料夾 | 用途 |
|----------|------|
| `reports/` | `/ask` 的研究報告與 `/lint` 的健康檢查報告 |
| `slides/` | 簡報輸出（預留給 Marp 等工具） |
| `charts/` | 圖表輸出（預留） |

**`scripts/`** — 預留給自訂 CLI 腳本或未來的 MCP server 實作。

**`.claude/commands/`** — Slash command 定義檔，是整個系統的「靈魂」。

| 檔案 | 用途 |
|------|------|
| `compile.md` | 編譯單一 raw 檔案 → summary + concepts |
| `ingest.md` | 自動偵測未處理的檔案，批次轉換 + compile |
| `index.md` | 重建 `wiki/index.md` 全域索引 |
| `ask.md` | 針對 wiki 內容進行研究型問答 |
| `lint.md` | wiki 健康檢查（連結、一致性、缺漏、tag） |

**`CLAUDE.md`** — 系統操作手冊，定義語言規則、目錄規則、連結規則、寫作風格，是 Claude Code 進入此專案時的行為準則。

## 三、LLM Engine 的五個工作流

整個系統的核心設計是五個獨立的工作流，各自有明確的觸發時機與輸入輸出：

| 功能 | slash command | 輸入 | 輸出 | 觸發時機 |
|------|-------------|------|------|---------|
| **Compile** | `/compile <path>` | raw 單檔 | summary + 新/更新的 concept 文章 | 手動指定素材 |
| **Ingest** | `/ingest` | 自動偵測 raw/ 中未處理檔案 | 批次轉換 + compile | 批次 ingest（支援 `--auto`） |
| **Index** | `/index` | 整個 wiki/ | 重建 `wiki/index.md` | 每次 compile 後 |
| **Ask** | `/ask <問題>` | wiki 全域 + raw 補充 | `outputs/reports/` 報告 | 隨時 |
| **Lint** | `/lint` | 整個 wiki/ | `outputs/reports/lint-*.md` | 定期（每週） |

### Compile — 單檔編譯

將一篇 raw 素材轉化為結構化知識的核心流程：

1. 讀取指定的 raw 檔案
2. 在 `wiki/summaries/` 建立摘要（300 字以內 + 3–5 關鍵論點），frontmatter 的 `source:` 指回原檔
3. 從素材中抽取 3–8 個關鍵概念
4. 對每個概念：若 `wiki/concepts/` 已有同名文章則補充新觀點，否則新建（至少 300 字）
5. summary 與 concept 之間用 `[[wiki-link]]` 互連
6. 自動呼叫 `/index` 更新索引

### Ingest — 批次偵測與編譯

解決「素材多了以後忘記哪些已經 compile 過」的問題，分三階段執行：

1. **盤點**：Glob 掃描 `raw/` 下所有 `.md` / `.pdf` / `.docx` / `.pptx` / `.html` / `.txt`，排除 `raw/repos/` 與隱藏檔。若 `X.pdf` 已有 `X.pdf.md` 則視為已轉換
2. **轉換**：對非 md 檔案進行格式轉換——學術論文用 `marker`（處理雙欄排版與公式），一般文件用 `markitdown`（輕量快速）。轉換後的 md 放回 raw/ 同目錄，命名為 `原檔名.副檔名.md`，並加入 frontmatter 標註原始來源與轉換日期
3. **比對並 compile**：以 `wiki/summaries/` 中 `source:` 欄位反查已處理清單，差集即為待 compile 檔案，逐一執行 `/compile`

### Index — 全域索引重建

讀取整個 `wiki/` 目錄，重新生成 `wiki/index.md`，內容包含：

- 統計資訊（concept / summary / exploration 數量、總字數、更新時間）
- 所有 concept 依 tag 分組，每篇附一句話說明
- 最近 10 篇新增文章
- 孤兒文章警告（沒有任何 backlink 的文章）

### Ask — 研究型問答

面對使用者的研究問題，系統性地從知識庫中組織答案：

1. 讀 `wiki/index.md` 掌握全域結構
2. 搜尋 wiki/ 中相關文章，讀取 3–8 篇
3. wiki 資訊不足時回溯 raw/ 原始素材補充
4. 產出報告存入 `outputs/reports/`，明確區分「wiki 已有的觀點」與「綜合推論」
5. 詢問使用者是否將有價值的答案 file 回 `wiki/explorations/`

### Lint — 知識庫健康檢查

定期對整個 wiki 進行五個面向的檢查，**只產出報告，不自動修改**：

1. **連結完整性**：找出指向不存在檔案的 wiki-link、找出孤兒文章
2. **內容一致性**：偵測同一概念的矛盾描述、重複文章
3. **缺漏資訊**：標記過短（< 200 字）或標註「待補」的文章
4. **新文章建議**：基於現有文章交集，建議 3–5 個值得新建的 concept
5. **Tag 整理**：列出所有 tag 使用次數，建議合併或拆分

## 四、實作中解決的關鍵問題

### 問題一：Obsidian Web Clipper 存放位置

**問題**：Web Clipper 預設把 clip 存到 vault 根目錄的 `Clippings/`，不會進 `raw/articles/`。

**解法**：在 Web Clipper Settings → Vaults → 對應 vault → Note location 改成 `raw/articles/`。進階可用多個 template 區分 articles / papers / repos。

### 問題二：自動找出未 compile 的檔案

**問題**：素材多了以後容易忘記哪些已經 compile 過。

**解法**：新增 `/ingest` slash command，利用 summary 檔 frontmatter 的 `source:` 欄位反查，找出 raw/ 中尚未被 compile 的檔案。

### 問題三：非 md 檔案（PDF、docx 等）的處理

**問題**：`/ingest` 最初只處理 md 檔，PDF 等其他格式不會被偵測。

**解法**：擴充成三階段流程（盤點 → 轉換 → compile），轉換後的 md 放回 raw/ 同目錄，命名為 `原檔名.副檔名.md`。好處是 raw/ 一目了然、比對邏輯統一、原始檔完整保留。

**轉換工具選擇**：學術論文用 `marker`（雙欄排版 + 公式友善），一般文件用 `markitdown`（輕量快速）。不直接用 Read 工具讀 PDF，因為轉換品質差異大。

## 五、日常使用流程

### 每次使用（30 分鐘典型 session）

```bash
# 1. 收集素材（Web Clipper 或手動放進 raw/）
# 2. 啟動 Claude Code
cd ~/knowledge-base && claude
```

在 Claude Code 裡：

```
/ingest          # 自動偵測、轉換、編譯所有新素材
```

完成後問一個真正想知道的問題：

```
/ask 綜合最近這幾篇資料，MCP server 在 multi-tenant 場景下最關鍵的三個安全風險是什麼？
```

有價值的答案會 file 回 `wiki/explorations/`。最後 commit：

```bash
git add -A && git commit -m "ingest and explore mcp security"
```

### 每週例行（週五下午）

```
/lint                 # 產出健康檢查報告
```

讀完報告，告訴 agent 採納哪些建議，讓它自動修。

## 六、使用心法

1. **餵料的品質決定一切**：不要為了累積數量而亂丟東西，垃圾進垃圾出。
2. **相信 agent 但要校對**：發現問題不要手動改 wiki，**改 prompt**。
3. **問問題比收集更重要**：每 ingest 5 篇就強迫自己 ask 1 個問題。
4. **所有 prompt 都要進 git**：`CLAUDE.md` 和 slash commands 是最有價值的資產。

## 七、與 MCP PaaS 業務的戰略連結

這個專案對 Joseph 的 MCP PaaS 業務有三層價值，應有意識地區分：

1. **個人生產力（立即）**：研究 LangGraph、MCP 生態、Block 四層架構的知識沉澱工具。
2. **產品驗證（中期）**：把 wiki search 寫成 local MCP server，親身體驗 MCP 開發者痛點，回饋到 PaaS 設計。
3. **商業機會（長期）**：「個人/組織知識庫 MCP」是真實 B2B 需求。但**先別為它做產品**，自用三個月累積 pain point 再判斷。

**最容易犯的錯**：第一週就想做第 3 層。先讓系統 boring 地為自己工作起來。

## 八、未來擴展方向

- **Wiki Search MCP Server**：把 wiki 搜尋包成 local MCP server，掛進 Claude Code，同時 dogfooding 自己的平台定位。
- **Marp 簡報輸出**：為客戶 demo / 內部分享產出簡報。
- **Manifest 升級**：從 frontmatter 反查升級到 `.compiled.json`，加上 file hash 偵測內容變更。
- **marker-pdf**：對圖多的重要論文改用 marker 保留圖表。
- **whisper.cpp**：處理 podcast 和影片逐字稿。
- **Synthetic data + finetune**（長期）：讓 LLM「知道」wiki 內容在權重裡，而不只是 context window。

---

## 附錄 A：關鍵檔案清單

| 檔案 | 用途 |
|------|------|
| `CLAUDE.md` | 系統操作手冊（語言、目錄、連結、寫作規範） |
| `.claude/commands/compile.md` | 單檔編譯 |
| `.claude/commands/ingest.md` | 批次處理新素材（含 PDF/docx 轉換） |
| `.claude/commands/index.md` | 全域索引維護 |
| `.claude/commands/ask.md` | 研究型問答 |
| `.claude/commands/lint.md` | 健康檢查 |
| `.gitignore` | 排除 `.DS_Store`、`.obsidian/`、`raw/repos/`、`outputs/` |

**所有 prompt 都是資產，務必進 git 版本控制。**

## 附錄 B：目前的知識庫規模

截至 2026-04-09 的實際狀態：

| 指標 | 數量 |
|------|------|
| raw/ 素材檔案 | ~1,416 個 |
| wiki/concepts/ | 86 篇 |
| wiki/summaries/ | 21 篇 |
| wiki/explorations/ | 2 篇 |
| 預估總字數 | ~10,600 字 |

### 已 compile 的素材來源

- **AI Engineering 全書**（共 10 章）— 產出 `ch01` 到 `ch10` 的 summary，是目前最大宗的知識來源
- **深度學習基礎到實作** — 產出 deep learning 相關概念群
- **Anthropic Economic Index: Learning Curves** — AI 對工作生產力影響的實證研究
- **AI 輔助對程式技能形成的影響** — 認知負荷與技能養成的研究
- **本地推論三件套**（Local LLM / MLX / Ollama / Quantization / GGUF）— 產出推論優化概念群
- **開放原始碼授權指南** — 開源授權的系統性整理
- **MemPalace 系列**（4 篇 summary）— Agent 記憶系統分類框架
- **生成式推薦系統** — LLM 在推薦系統中的應用調查
- **學術論文** — `raw/papers/` 中有 2 篇 PDF，其中 1 篇已轉換為 md

### 概念文章覆蓋的主題叢集

- Agent Memory 與認知架構（7 篇）
- AI Agent 與工具使用（3 篇）
- 基礎模型與架構（6 篇）
- Prompt Engineering（6 篇）
- 評估與品質（9 篇）
- 訓練技術（8 篇）
- 推論與部署（7 篇）
- RAG 與資訊檢索（4 篇）
- 推薦系統與 AI（5 篇）
- AI 與人類協作（5 篇）
- 神經網路基礎（8 篇）
- 其他（資料、開源授權等）

### 完成標準與達成狀況

| 指標 | 目標 | 實際狀況 |
|------|------|---------|
| 餵入素材後 wiki 自動長出概念文章 | 5 篇素材 → 10–20 篇 concept | ✅ 21 篇 summary → 86 篇 concept |
| 任一 concept 有 wiki-link 連到其他文章 | 至少 1 個連結 | ✅ 所有文章皆有 Related 區塊 |
| Obsidian graph view 有連線 | 節點間有連線 | ✅ wiki-link 交叉引用密度高 |

**三項指標全數達成。** 系統已進入日常運轉階段。
