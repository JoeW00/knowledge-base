# Ollama 實戰指南

> 這份指南幫助你掌握 Ollama — 在本地跑大型語言模型最簡單的工具。
> 讀完後你將能安裝、管理、使用本地模型，並了解常用的效能調校方法。

---

## 目錄

- [1. 什麼是 Ollama？](#1-什麼是-ollama)
- [2. Ollama 的架構](#2-ollama-的架構)
- [3. 安裝](#3-安裝)
- [4. 基本操作](#4-基本操作)
- [5. 模型管理](#5-模型管理)
- [6. 進階設定與效能調校](#6-進階設定與效能調校)
- [7. API 使用](#7-api-使用)
- [8. 搭配其他工具使用](#8-搭配其他工具使用)
- [9. 常見問題排查](#9-常見問題排查)

---

## 1. 什麼是 Ollama？

Ollama 是一個**一站式的本地模型管理工具**。它把下載模型、載入記憶體、GPU 加速、對話互動全部包裝好，讓你一行指令就能跑模型：

```bash
ollama run qwen3
```

就這樣。不用自己下載模型檔、不用設定 GPU、不用寫任何程式碼。

### Ollama 的定位

想像你要喝咖啡：
- **自己烘豆、研磨、手沖** = 用 llama.cpp 從原始模型開始設定
- **用全自動咖啡機** = 用 Ollama，按一個鍵就出咖啡

Ollama 就是本地 AI 的「全自動咖啡機」。

---

## 2. Ollama 的架構

```
┌──────────────────────────────────┐
│  你（終端機 / API 客戶端）        │
├──────────────────────────────────┤
│  Ollama CLI / REST API            │  ← 使用者介面
├──────────────────────────────────┤
│  Ollama Server（背景服務）        │  ← 管理模型生命週期
├──────────────────────────────────┤
│  llama.cpp（推理引擎）            │  ← 實際做計算
├──────────────────────────────────┤
│  Metal / CUDA                     │  ← GPU 運算框架
├──────────────────────────────────┤
│  GPU 硬體                         │
└──────────────────────────────────┘
```

關鍵理解：
- Ollama **不是**推理引擎本身，它底層用的是 **llama.cpp**
- Ollama 的價值在於**簡化操作** — 模型下載、記憶體管理、GPU 分配都自動處理
- Ollama 以背景服務（daemon）的形式運行，你的指令透過 API 跟它溝通

---

## 3. 安裝

### macOS

```bash
# 方法一：官網下載
# 前往 https://ollama.com 下載 macOS 版

# 方法二：Homebrew
brew install ollama
```

安裝完成後，Ollama 會自動偵測你的硬體（M 系列晶片會使用 Metal 加速）。

### 驗證安裝

```bash
ollama --version
```

### 啟動服務

macOS 上安裝後通常會自動啟動。如果沒有：

```bash
ollama serve
```

---

## 4. 基本操作

### 跑模型（最常用）

```bash
ollama run gemma3
```

第一次執行會自動下載模型，之後就直接載入。進入對話模式後：
- 直接打字就能對話
- 輸入 `/bye` 退出對話
- 輸入 `"""` 可以多行輸入

### 一次性提問（不進入對話模式）

```bash
ollama run gemma3 "用一句話解釋量化是什麼"
```

### 查看目前已安裝的模型

```bash
ollama list
```

輸出範例：
```
NAME              ID            SIZE    MODIFIED
gemma3:latest     a2af6cc3eb7f  5.4 GB  2 days ago
qwen3:latest      ...           ...     ...
```

### 查看正在運行的模型

```bash
ollama ps
```

輸出會顯示模型佔用的記憶體和使用的運算設備（GPU/CPU）。

---

## 5. 模型管理

### 下載模型

```bash
ollama pull llama3.2
```

可以指定版本標籤：
```bash
ollama pull qwen3:32b        # 指定 32B 參數版本
ollama pull qwen3:72b-q4_K_M # 指定量化等級
```

### 刪除模型

```bash
ollama rm gemma3
```

### 查看模型詳細資訊

```bash
ollama show gemma3
```

顯示模型的架構、參數量、量化等級、上下文長度等資訊。

### 複製模型（建立別名）

```bash
ollama cp llama3.2 gpt-3.5-turbo
```

這在搭配需要特定模型名稱的工具時很有用（例如某些工具硬編碼要求 `gpt-3.5-turbo`）。

### 使用自訂 GGUF 模型

如果你從 Hugging Face 下載了一個 GGUF 檔案，可以建立一個 `Modelfile`：

```
# Modelfile
FROM /path/to/your-model.gguf

PARAMETER temperature 0.7
PARAMETER num_ctx 4096

SYSTEM "你是一個有幫助的助手。"
```

然後建立模型：
```bash
ollama create my-model -f Modelfile
ollama run my-model
```

---

## 6. 進階設定與效能調校

### 控制模型在記憶體中的存留時間

模型載入記憶體後，預設會保留 5 分鐘才卸載。你可以調整：

```bash
# 讓模型一直留在記憶體中（不自動卸載）
curl http://localhost:11434/api/generate -d '{"model": "qwen3", "keep_alive": -1}'

# 立即卸載模型
curl http://localhost:11434/api/generate -d '{"model": "qwen3", "keep_alive": 0}'

# 或用 CLI 停止模型
ollama stop qwen3
```

### 設定上下文長度

```bash
# 在 Modelfile 中設定
PARAMETER num_ctx 8192
```

上下文越長，模型能「記住」的對話越多，但記憶體佔用也越大。

### 環境變數

| 環境變數 | 說明 | 預設值 |
|---|---|---|
| `OLLAMA_HOST` | Ollama 服務的監聽位址 | `127.0.0.1:11434` |
| `OLLAMA_MODELS` | 模型儲存路徑 | `~/.ollama/models` |
| `OLLAMA_NUM_PARALLEL` | 同時處理的請求數 | 1 |
| `OLLAMA_MAX_LOADED_MODELS` | 同時載入記憶體的模型數 | 1 |

設定方式（macOS）：
```bash
# 在 ~/.zshrc 或 ~/.bashrc 中加入
export OLLAMA_HOST="0.0.0.0:11434"  # 允許區網其他裝置存取
```

---

## 7. API 使用

Ollama 提供 REST API，其他程式可以透過 HTTP 呼叫來使用模型。

### 生成回應

```bash
curl http://localhost:11434/api/generate -d '{
  "model": "qwen3",
  "prompt": "什麼是量化？",
  "stream": false
}'
```

### 對話（含歷史）

```bash
curl http://localhost:11434/api/chat -d '{
  "model": "qwen3",
  "messages": [
    {"role": "user", "content": "什麼是量化？"}
  ],
  "stream": false
}'
```

### OpenAI 相容 API

Ollama 也提供 OpenAI 格式的 API，讓原本用 OpenAI 的程式碼幾乎不用改就能切換到本地模型：

```python
from openai import OpenAI

client = OpenAI(
    base_url="http://localhost:11434/v1",
    api_key="ollama"  # 任意值即可
)

response = client.chat.completions.create(
    model="qwen3",
    messages=[{"role": "user", "content": "什麼是量化？"}]
)
print(response.choices[0].message.content)
```

這個相容性非常實用 — 你可以用本地模型替代 OpenAI API 做開發和測試，不用付費。

---

## 8. 搭配其他工具使用

### Open WebUI

一個網頁介面，讓你用類似 ChatGPT 的 UI 跟本地模型對話。

```bash
# 用 Docker 啟動
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  --name open-webui \
  ghcr.io/open-webui/open-webui:main
```

然後在瀏覽器開啟 `http://localhost:3000`，它會自動連接本地的 Ollama。

### Continue（VS Code 擴充套件）

在 VS Code 裡用本地模型做程式碼補全和對話，設定 Ollama 為後端即可。

### 自己的 Python 程式

```python
import requests

response = requests.post("http://localhost:11434/api/generate", json={
    "model": "qwen3",
    "prompt": "寫一個 Python 的 hello world",
    "stream": False
})
print(response.json()["response"])
```

---

## 9. 常見問題排查

### 模型下載很慢

**原因**：模型檔案很大（數 GB 到數十 GB），下載速度取決於網路。

**解法**：
- 確認網路連線穩定
- 嘗試在離峰時段下載
- 如果有 GGUF 檔案，可以用 `Modelfile` 直接從本地載入

### 記憶體不足 / 系統卡頓

**原因**：模型佔用了太多記憶體，系統開始用 swap。

**解法**：
1. `ollama stop <model>` 卸載模型
2. 關掉佔記憶體的程式後再試
3. 改用更小的模型或更低的量化等級
4. 減少上下文長度（`num_ctx`）

### 模型跑起來很慢

**排查步驟**：
1. 用 `ollama ps` 確認 Run Mode 是 GPU 還是 CPU
2. 如果是 CPU，表示記憶體不夠讓模型完全載入 GPU
3. 解法：換更小的模型 / 更低量化 / 清出更多記憶體

### Ollama 服務沒啟動

**症狀**：`Error: could not connect to ollama app`

**解法**：
```bash
# 手動啟動
ollama serve

# 確認是否在運行
curl http://localhost:11434
# 應回傳 "Ollama is running"
```

### 想用的模型在 Ollama 上找不到

Ollama 的模型庫不是所有模型都有。如果找不到：
1. 去 Hugging Face 下載 GGUF 檔案
2. 建立 `Modelfile` 指向該檔案
3. 用 `ollama create` 匯入

---

## 相關指南

- [本地 LLM 推理入門指南](Local_LLM_Inference_Guide.md) — 本地推理的基礎概念
- [模型量化與 GGUF 格式指南](Quantization_GGUF_Guide.md) — 理解量化等級和模型格式
- [MLX 入門指南](MLX_Guide.md) — Apple Silicon 上的替代推理引擎
