---
title: MLX
tags: [local-inference, apple-silicon, framework, deep-learning]
created: 2026-04-09
updated: 2026-04-09
---

# MLX

MLX 是 Apple 於 2023 年底推出的機器學習框架，專為 Apple Silicon（M1-M4 系列）設計。其定位等同於 Apple 生態的 PyTorch：上層框架（MLX）→ 底層平台（Metal）→ 硬體（Apple Silicon GPU），對應 NVIDIA 生態的 PyTorch → CUDA → NVIDIA GPU。

## 為何 Apple 要自己做框架

AI 生態幾乎完全建立在 NVIDIA CUDA 之上，PyTorch 與 TensorFlow 對 Apple Metal 的支援僅止於「能用」。Apple 選擇從底層針對自家晶片重新設計，而非在他人框架上打補丁。MLX 的 API 刻意仿照 NumPy 與 PyTorch 的風格，降低遷移成本。

## 核心優勢

**統一記憶體零拷貝**是 MLX 最大的技術優勢。Apple Silicon 的 CPU 與 GPU 共享同一塊記憶體，MLX 利用延遲求值（lazy evaluation）讓張量在 CPU 與 GPU 之間無需複製即可共用，消除了傳統架構中 RAM↔VRAM 的資料傳輸瓶頸。這讓 64 GB 統一記憶體的 Mac 能載入比同等 VRAM 的 NVIDIA GPU 更大的模型。

**mlx-lm** 是 MLX 的 LLM 推理與微調工具，支援直接從 Hugging Face 下載模型、量化轉換、推理生成，以及用極少程式碼完成 LoRA 微調——這是 [[ollama]] 路線目前不支援的功能。

## 適用場景

在 Mac 上追求最佳推理效能、需要在本地做 LoRA 微調、或希望以 Python 靈活控制推理流程的使用者適合 MLX。若只需簡單跑模型對話，Ollama 的便利性更勝一籌。

## Related

- [[local-llm-inference]]
- [[ollama]]
- [[quantization]]
- [[lora]]
- [[local-llm-guides-summary]]
