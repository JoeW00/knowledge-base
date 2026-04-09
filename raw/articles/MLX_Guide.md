# MLX 入門指南

> 這份指南幫助你理解 MLX — Apple 為自家晶片打造的機器學習框架。
> 讀完後你將了解 MLX 的定位、它跟 Ollama/llama.cpp 的差異、以及什麼時候該用它。

---

## 目錄

- [1. 什麼是 MLX？](#1-什麼是-mlx)
- [2. MLX 在軟體堆疊中的位置](#2-mlx-在軟體堆疊中的位置)
- [3. MLX vs Ollama/llama.cpp](#3-mlx-vs-ollamallama-cpp)
- [4. MLX 的核心特性](#4-mlx-的核心特性)
- [5. 安裝與環境設定](#5-安裝與環境設定)
- [6. 用 mlx-lm 跑模型推理](#6-用-mlx-lm-跑模型推理)
- [7. 用 MLX 做 LoRA 微調](#7-用-mlx-做-lora-微調)
- [8. MLX vs PyTorch 在 Mac 上的選擇](#8-mlx-vs-pytorch-在-mac-上的選擇)
- [9. 常見問題排查](#9-常見問題排查)

---

## 1. 什麼是 MLX？

MLX 是 Apple 在 2023 年底推出的**機器學習框架**，專門為 Apple Silicon（M1/M2/M3/M4）設計。

用一句話來說：**MLX 之於 Apple Silicon，就像 PyTorch 之於 NVIDIA GPU。**

它讓你用 Python 寫幾行程式碼就能在 Mac 上高效地跑 AI 模型，不用操心底層的 Metal GPU 程式設計。

### MLX 的由來

Apple 有自己的 GPU（M 系列晶片）和自己的 GPU 運算框架（Metal），但整個 AI 生態系都建立在 NVIDIA CUDA 上。PyTorch、TensorFlow 原生支援 CUDA，對 Metal 的支援只是「能用」的程度。

Apple 決定自己做一個框架，從底層就針對自家晶片最佳化，而不是在別人的框架上打補丁。這就是 MLX。

---

## 2. MLX 在軟體堆疊中的位置

```
┌─────────────────────────────────────────────┐
│  你的程式 / mlx-lm CLI                       │  ← Python 程式碼
├─────────────────────────────────────────────┤
│  MLX 框架                                    │  ← 張量運算、自動微分
│  （等同於 PyTorch 的角色）                    │
├─────────────────────────────────────────────┤
│  Metal                                       │  ← GPU 運算框架
│  （等同於 CUDA 的角色）                       │
├─────────────────────────────────────────────┤
│  Apple Silicon GPU                            │  ← 硬體
└─────────────────────────────────────────────┘
```

對照表：

| 層級 | Apple 生態系 | NVIDIA 生態系 |
|---|---|---|
| 上層框架 | **MLX** | PyTorch |
| 底層平台 | Metal | CUDA |
| 硬體 | M 系列 GPU | NVIDIA GPU |

**Metal** 負責跟 GPU 硬體溝通（底層、繁瑣）。**MLX** 讓你用 Python 高階操作就能驅動 GPU（上層、簡潔）。你平常只需要跟 MLX 打交道，Metal 在背後自動運作。

---

## 3. MLX vs Ollama/llama.cpp

這是最常見的疑問：「我已經有 Ollama 了，為什麼還要裝 MLX？」

### 它們是不同層級的東西

```
路線 A（你目前的方式）：
    Ollama（使用者介面） → llama.cpp（推理引擎） → Metal → GPU

路線 B（MLX 方式）：
    mlx-lm（使用者介面） → MLX（推理引擎） → Metal → GPU
```

兩條路都能跑模型，都用 Metal 驅動 GPU，差別在中間的推理引擎。

### 具體差異

| 面向 | Ollama（llama.cpp） | MLX（mlx-lm） |
|---|---|---|
| **易用性** | 極高 — 一行指令 | 中等 — 需要 Python 環境 |
| **Apple Silicon 最佳化** | 好 | 更好（專為 Apple 設計） |
| **統一記憶體利用** | 好 | 更好（原生支援） |
| **推理速度（Mac 上）** | 快 | 通常更快 |
| **模型格式** | GGUF | Safetensors（Hugging Face 原生格式） |
| **跨平台** | 支援 Linux、Windows、Mac | 僅 Mac（Apple Silicon） |
| **可程式化** | 透過 API | 原生 Python，彈性更大 |
| **微調能力** | 不支援 | 支援（LoRA） |
| **社群大小** | 很大 | 較小但成長中 |

### 什麼時候該用 MLX？

- **追求最佳速度**：MLX 在 Apple Silicon 上通常比 llama.cpp 快 10-30%
- **想用 Python 整合**：直接在 Python 程式中呼叫模型，不用透過 HTTP API
- **想做微調**：MLX 支援 LoRA 微調，Ollama 不支援
- **想學習框架底層**：MLX 的程式碼比 PyTorch 簡單，適合學習

### 什麼時候用 Ollama 就好？

- **只想對話**：Ollama 最簡單，一行指令搞定
- **需要跨平台**：MLX 只能在 Mac 上用
- **不想碰 Python**：Ollama 不需要任何程式設計

---

## 4. MLX 的核心特性

### 統一記憶體（Unified Memory）

這是 MLX 最重要的特性。在 MLX 中，陣列（array）自動存放在統一記憶體中，CPU 和 GPU 都能直接存取，不需要複製：

```python
import mlx.core as mx

# 建立的陣列同時可被 CPU 和 GPU 存取
a = mx.random.normal((100,))
b = mx.random.normal((100,))

# 不需要像 PyTorch 那樣寫 .to("cuda") 或 .to("mps")
c = a + b  # 自動在最適合的設備上執行
```

對比 PyTorch，你需要明確搬移資料：
```python
# PyTorch 的寫法（需要手動搬移）
a = torch.randn(100).to("mps")  # 搬到 GPU
b = torch.randn(100).to("mps")
c = a + b
```

### 延遲計算（Lazy Evaluation）

MLX 不會立即執行運算，而是先記錄「要做什麼」，等你真正需要結果時才一次算完。好處是可以最佳化計算順序、節省記憶體：

```python
import mlx.core as mx

# 這行不會立即執行計算
a = mx.ones((1000, 1000))
b = mx.ones((1000, 1000))
c = a + b  # 只是記錄了「a + b」這個操作

# 直到你呼叫 eval() 或需要看結果時，才真正計算
mx.eval(c)
```

這在載入模型時特別有用 — 模型物件建立時不佔記憶體，直到真正載入權重時才分配：

```python
model = Model()  # 還沒用到記憶體
model.load_weights("weights.safetensors")  # 這時才分配記憶體
```

### 可組合的函數轉換

MLX 提供像 JAX 一樣的函數轉換，對進階使用者很強大：

```python
import mlx.core as mx
import mlx.nn as nn

# 自動計算梯度
grad_fn = mx.grad(loss_function)

# 自動向量化
vmap_fn = mx.vmap(single_example_fn)

# JIT 編譯加速
compiled_fn = mx.compile(compute_fn)
```

---

## 5. 安裝與環境設定

### 系統需求

- Apple Silicon Mac（M1 或更新）
- macOS 13.5 或更新
- Python 3.9 或更新

### 安裝 MLX

```bash
pip install mlx
```

### 安裝 mlx-lm（用來跑語言模型）

```bash
pip install mlx-lm
```

`mlx-lm` 是 MLX 生態系中專門用於跑大型語言模型的工具，提供 CLI 和 Python API。

### 驗證安裝

```python
import mlx.core as mx
print(mx.default_device())  # 應顯示 gpu
print(mx.metal.is_available())  # 應顯示 True
```

---

## 6. 用 mlx-lm 跑模型推理

### 用 CLI 跑模型

```bash
# 從 Hugging Face 下載並跑模型（第一次會自動下載）
mlx_lm.generate \
  --model mlx-community/Qwen2.5-7B-Instruct-4bit \
  --prompt "什麼是量化？"
```

### 用 Python 跑模型

```python
from mlx_lm import load, generate

# 載入模型（會自動下載）
model, tokenizer = load("mlx-community/Qwen2.5-7B-Instruct-4bit")

# 生成回應
response = generate(
    model,
    tokenizer,
    prompt="什麼是量化？",
    max_tokens=256
)
print(response)
```

### 互動式對話

```bash
mlx_lm.chat --model mlx-community/Qwen2.5-7B-Instruct-4bit
```

### 模型來源

MLX 使用的模型格式跟 Ollama 不同：
- **Ollama** 用 GGUF 格式
- **MLX** 用 Safetensors 格式（Hugging Face 原生格式）

在 Hugging Face 上，搜尋 `mlx-community` 可以找到已經轉換好的 MLX 格式模型。如果找不到，也可以自己轉換：

```bash
mlx_lm.convert \
  --hf-path Qwen/Qwen2.5-7B-Instruct \
  --mlx-path ./Qwen2.5-7B-mlx \
  --quantize --q-bits 4
```

---

## 7. 用 MLX 做 LoRA 微調

### 什麼是 LoRA？

**LoRA（Low-Rank Adaptation）** 是一種「輕量微調」技術。它不修改模型的原始參數，而是在旁邊加上一小組新的參數來調整模型行為。

好處：
- 不需要全部重新訓練（原始的 79.7B 參數不動）
- 新增的參數很小（通常只有幾 MB）
- 一台 Mac 就能做

### 用 mlx-lm 做 LoRA 微調

```bash
mlx_lm.lora \
  --model mlx-community/Qwen2.5-7B-Instruct-4bit \
  --train \
  --data ./training_data \
  --iters 1000
```

訓練資料格式（JSONL）：
```json
{"text": "<s>[INST] 問題 [/INST] 回答</s>"}
{"text": "<s>[INST] 問題 [/INST] 回答</s>"}
```

### 這跟「從零訓練」不同

| 方式 | 修改的參數量 | 需要的資源 | 適合場景 |
|---|---|---|---|
| 從零預訓練 | 全部（79.7B） | 數千張 GPU | 只有大公司做得到 |
| 全量微調 | 全部（79.7B） | 數張 A100 GPU | 研究機構 |
| **LoRA 微調** | **數百萬（<1%）** | **一台 Mac** | **個人開發者** |

MLX 讓你在 Mac 上就能做 LoRA 微調，這是 Ollama 做不到的事。

---

## 8. MLX vs PyTorch 在 Mac 上的選擇

### PyTorch 在 Mac 上也能用 GPU

PyTorch 透過 `device = "mps"`（Metal Performance Shaders）可以在 Mac 上使用 GPU 加速：

```python
import torch

device = torch.device("mps")  # 使用 Apple GPU
tensor = torch.randn(100).to(device)
```

所以**不是**「用 PyTorch 就只能用 CPU」。

### 那為什麼還需要 MLX？

| 面向 | PyTorch + MPS | MLX |
|---|---|---|
| 統一記憶體利用 | 有支援，但不是原生設計 | 原生設計，最佳化更深 |
| Apple Silicon 效能 | 好 | 更好（通常快 20-50%） |
| 生態系 | 龐大，幾乎所有 AI 資源 | 小，但快速成長 |
| 學習資源 | 極多 | 較少 |
| API 設計 | 成熟但複雜 | 類似 NumPy，更簡潔 |
| 用途廣度 | 訓練 + 推理 + 研究 | 主要用於推理和輕量微調 |

### 務實建議

| 你想做的事 | 建議用 |
|---|---|
| 跑別人訓練好的模型（推理） | MLX（速度最快） |
| 在 Mac 上做 LoRA 微調 | MLX（原生支援） |
| 學習機器學習、跟教程 | PyTorch（資源多） |
| 做研究、寫論文 | PyTorch（學術標準） |
| 需要跨平台（Mac + Linux） | PyTorch（MLX 只支援 Mac） |

---

## 9. 常見問題排查

### 安裝失敗

**症狀**：`pip install mlx` 報錯。

**檢查**：
1. 確認是 Apple Silicon Mac（Intel Mac 不支援）
2. 確認 macOS 版本 ≥ 13.5
3. 確認 Python 版本 ≥ 3.9
4. 嘗試 `pip install --upgrade pip` 後重試

### 模型下載很慢

**解法**：MLX 模型從 Hugging Face 下載，速度取決於網路。可以先用 `huggingface-cli download` 下載到本地快取。

### 記憶體不足

**症狀**：載入模型時系統卡頓或報錯。

**解法**：
1. 用更小的模型或更低的量化等級
2. 關掉其他應用程式
3. 減少 `max_tokens` 和上下文長度

### mlx-community 上找不到想要的模型

**解法**：自己轉換。用 `mlx_lm.convert` 從 Hugging Face 的原始模型轉成 MLX 格式：

```bash
mlx_lm.convert \
  --hf-path <Hugging Face 模型名稱> \
  --mlx-path <本地輸出路徑> \
  --quantize --q-bits 4
```

### MLX 跑起來沒有比 Ollama 快

可能的原因：
- 模型太小（差異在大模型上才明顯）
- 系統記憶體壓力大（兩者都受影響）
- 不同模型的 MLX 最佳化程度不同

---

## 相關指南

- [本地 LLM 推理入門指南](Local_LLM_Inference_Guide.md) — 本地推理的基礎概念
- [模型量化與 GGUF 格式指南](Quantization_GGUF_Guide.md) — 理解量化等級和模型格式
- [Ollama 實戰指南](Ollama_Guide.md) — 最簡單的本地推理工具
