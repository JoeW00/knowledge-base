---
title: Ollama
tags: [local-inference, tooling, llm, deployment]
created: 2026-04-09
updated: 2026-04-09
---

# Ollama

Ollama 是一套一站式的本地模型管理工具，將模型下載、記憶體載入、GPU 加速、對話互動全部封裝為簡單的 CLI 與 REST API，讓使用者一行指令（`ollama run qwen3`）就能在本機運行大型語言模型。

## 架構

Ollama 本身不是推理引擎，它是建構在 **llama.cpp** 之上的管理層。完整堆疊為：CLI/API → Ollama Server（背景服務）→ llama.cpp（實際推理）→ Metal/CUDA → GPU 硬體。Ollama 的價值在於自動化了模型下載、格式轉換、GPU 分配與記憶體管理，大幅降低了 [[local-llm-inference]] 的入門門檻。

## 核心功能

模型管理方面，Ollama 維護自己的模型倉庫（ollama.com/library），支援 `pull`/`list`/`rm` 等操作，也能透過 Modelfile 從 GGUF 檔案匯入自訂模型。效能調校方面，可控制 GPU offload 層數、context 長度、以及並行請求數。API 方面，Ollama 提供 OpenAI 相容的 REST API（`/api/generate`、`/api/chat`），可與 Open WebUI、Continue 等第三方工具無縫整合。

## 與 MLX 的比較

Ollama 與 [[mlx]] 是兩條平行的本地推理路線。Ollama 透過 llama.cpp 支援多平台（macOS、Linux、Windows），且模型生態最豐富；MLX 僅限 Apple Silicon 但對其硬體深度最佳化，在 Mac 上通常有更好的推理效能。對於多數使用者，Ollama 的簡便性是最大優勢；對於需要在 Mac 上微調或追求極致效能的使用者，MLX 更為合適。

## Related

- [[local-llm-inference]]
- [[mlx]]
- [[quantization]]
- [[local-llm-guides-summary]]
