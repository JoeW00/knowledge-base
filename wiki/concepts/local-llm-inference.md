---
title: 本地 LLM 推理 (Local LLM Inference)
tags: [inference, local-first, hardware, deployment]
created: 2026-04-09
updated: 2026-04-09
---

# 本地 LLM 推理 (Local LLM Inference)

本地 LLM 推理是指在個人電腦上運行大型語言模型的完整推理流程，不依賴雲端 API，資料完全不離開本機。隨著模型 [[quantization]] 技術的成熟與 Apple Silicon 統一記憶體架構的普及，在消費級硬體上運行 70B+ 參數的模型已成為現實。

## 核心瓶頸：記憶體

本地推理的首要限制不是運算速度，而是**記憶體容量**。模型權重必須完整（或大部分）載入記憶體才能運行。一個 70B 參數的 FP16 模型需要 ~140 GB，但經過 Q4_K_M 量化後僅需 ~36 GB，讓 64 GB 記憶體的機器也能運行。推理速度以 tok/s（每秒生成 token 數）衡量，10-20 tok/s 即可達到流暢的對話體驗。

Apple Silicon 的統一記憶體架構在此場景具有獨特優勢：CPU 和 GPU 共享同一塊記憶體，不需要在 CPU RAM 和 GPU VRAM 之間拷貝資料。這讓 M 系列 Mac 能將全部記憶體用於模型載入，而 NVIDIA 方案則受限於獨立 VRAM 容量。

## 軟體堆疊

本地推理的技術棧分為三層：底層 GPU 運算框架（CUDA for NVIDIA、Metal for Apple）、中層推理引擎（llama.cpp、[[mlx]]）、上層使用者介面（[[ollama]]、mlx-lm CLI、Open WebUI）。兩條主流路線：Ollama 路線（Ollama → llama.cpp → Metal/CUDA）簡單易用，一行指令即可運行；MLX 路線（mlx-lm → MLX → Metal）對 Apple 硬體深度最佳化，且支援 LoRA 微調。

## 適用場景

處理機密或敏感資料（不上傳雲端）、大量重複性任務（省去 API 費用）、離線環境、以及 AI 實驗與學習。相較雲端 API，本地推理犧牲了模型品質上限與推理速度，但獲得了完全的資料隱私與零費用。

## Related

- [[quantization]]
- [[ollama]]
- [[mlx]]
- [[inference-optimization]]
- [[local-llm-guides-summary]]
