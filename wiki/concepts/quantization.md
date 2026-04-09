---
title: "Quantization"
tags: [optimization, inference, training, efficiency]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Quantization（量化）是一種模型壓縮技術，透過降低模型參數的數值精度來減少記憶體佔用並提升推理速度。典型的量化路徑為 FP32 → FP16 → INT8 → INT4，每一級約將記憶體需求減半。量化是讓大型語言模型在消費級硬體上運行的關鍵技術之一。

## 數值格式

不同的數值格式在精度與效率之間提供不同的權衡。FP32（32 位元浮點）是傳統訓練格式，提供最高精度但佔用最多記憶體。FP16 與 BF16 將精度減半，是目前主流的訓練與推理格式。INT8 與 INT4 則進一步壓縮，主要用於推理階段，能在可接受的品質損失下大幅提升效能。NF4（NormalFloat4）是 [[qlora]] 引入的特殊 4-bit 格式，針對常態分佈的權重進行最佳化。

## Post-Training Quantization vs. Quantization-Aware Training

Post-training quantization（PTQ）在模型訓練完成後直接將權重轉換為低精度格式，操作簡單但可能造成較大的精度損失。Quantization-aware training（QAT）則在訓練過程中模擬量化效果，讓模型學會適應低精度運算，通常能獲得更好的量化品質，但需要額外的訓練成本。

## 主流方法

GPTQ 是廣泛使用的 post-training 量化方法，透過逐層最小化量化誤差來保持模型品質。AWQ（Activation-Aware Weight Quantization）則根據激活值的分佈來決定量化策略，對重要的權重通道保留更高精度。兩者都能將模型壓縮至 4-bit 並維持良好的生成品質。

## 對模型品質的影響

量化不可避免地會造成一定程度的精度損失，但現代方法已將此影響控制在相當可接受的範圍內。一般而言，8-bit 量化幾乎不影響模型表現，4-bit 量化在大多數任務上也能維持接近原始精度九成以上的效能。模型越大，對量化的耐受度越高。

## GGUF 格式與本地推理

在 [[local-llm-inference]] 場景中，GGUF 是 llama.cpp 生態系的標準模型格式，將量化後的權重與 tokenizer 等元資料封裝為單一檔案。llama.cpp 的 K-quant 系列（Q2_K 到 Q8_0）提供從極端壓縮到近乎無損的細膩等級選擇。實務經驗顯示，**Q4_K_M** 是多數場景的最佳平衡點：保留 95-97% 品質，一個 70B 模型可從 140 GB 壓縮至 ~36 GB，讓 64 GB 記憶體的消費級電腦也能運行。

## 應用場景

量化在三個主要場景發揮作用：雲端推理最佳化（減少服務成本與延遲）、本地推理（讓大模型在消費級硬體上運行，見 [[local-llm-inference]]）、以及微調（如 [[qlora]] 使消費級 GPU 也能微調大模型）。在生產部署與本地推理中，量化幾乎已成為標準步驟。

## Related

- [[qlora]]
- [[fine-tuning]]
- [[lora]]
- [[peft]]
- [[local-llm-inference]]
- [[ollama]]
- [[mlx]]
- [[local-llm-guides-summary]]
