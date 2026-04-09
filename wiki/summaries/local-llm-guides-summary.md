---
title: 本地 LLM 推理系列指南摘要
source:
  - raw/articles/Local_LLM_Inference_Guide.md
  - raw/articles/Quantization_GGUF_Guide.md
  - raw/articles/Ollama_Guide.md
  - raw/articles/MLX_Guide.md
tags: [local-inference, quantization, ollama, mlx, hardware]
created: 2026-04-09
updated: 2026-04-09
---

# 本地 LLM 推理系列指南摘要

四篇教學指南組成一套完整的本地 LLM 推理入門體系，從硬體概念到實際操作，幫助讀者在自己的電腦上運行大型語言模型。

**《本地 LLM 推理入門指南》** 建立核心概念框架：推理 vs. 訓練的區別、本地 vs. 雲端 API 的取捨（隱私、成本、離線 vs. 速度、品質）、GPU 運算框架（CUDA vs. Metal）、統一記憶體 vs. 獨立 VRAM 的差異、MoE 架構如何讓大模型在消費級硬體上運行，以及記憶體需求的計算方法。

**《模型量化與 GGUF 格式指南》** 深入 [[quantization]] 的實務面：從 FP16 到 Q2_K 各等級的品質 vs. 空間權衡、K-quant 命名規則解讀、GGUF 格式作為 llama.cpp 生態標準的角色，以及依硬體記憶體選擇量化等級的決策流程。核心洞見：Q4_K_M 是多數場景的最佳平衡點，保留 95-97% 品質且大幅壓縮模型體積。

**《Ollama 實戰指南》** 涵蓋 [[ollama]] 的完整操作：架構（CLI → Server → llama.cpp → Metal/CUDA）、模型管理（下載、列表、刪除）、效能調校（GPU offload 層數、context 長度）、REST API 使用，以及與 Open WebUI 等工具的整合。

**《MLX 入門指南》** 介紹 Apple 為 Apple Silicon 打造的 [[mlx]] 框架：與 PyTorch/CUDA 的對照定位、與 Ollama/llama.cpp 的差異（不同推理引擎路線）、統一記憶體零拷貝優勢、mlx-lm 推理與 LoRA 微調操作，以及何時該選 MLX 而非 Ollama。

## 關鍵論點

1. 本地推理的核心瓶頸是**記憶體容量**而非運算速度——模型必須完整載入記憶體才能運行
2. 量化是讓大模型塞進消費級硬體的關鍵技術，Q4_K_M 為實務首選
3. Apple Silicon 的統一記憶體架構讓 Mac 成為本地推理的優勢平台
4. Ollama 與 MLX 代表兩條平行的推理路線，前者簡單易用，後者對 Apple 硬體深度最佳化

## 原文路徑

- `raw/articles/Local_LLM_Inference_Guide.md`
- `raw/articles/Quantization_GGUF_Guide.md`
- `raw/articles/Ollama_Guide.md`
- `raw/articles/MLX_Guide.md`

## Related

- [[local-llm-inference]]
- [[quantization]]
- [[ollama]]
- [[mlx]]
