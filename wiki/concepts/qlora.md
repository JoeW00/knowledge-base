---
title: "QLoRA"
tags: [fine-tuning, quantization, efficiency]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

QLoRA 是結合 4-bit [[quantization]] 與 [[lora]] 的微調方法，由 Dettmers 等人於 2023 年提出。它讓研究者與開發者能在消費級 GPU 上微調數百億參數的大型語言模型，大幅降低了 [[fine-tuning]] 的硬體門檻。最具代表性的成就是：單張 48GB GPU 即可微調 650 億參數模型。

## 核心創新

QLoRA 引入了三項關鍵技術。第一是 NF4（NormalFloat4）資料類型，這是一種針對常態分佈權重最佳化的 4-bit 格式，相較於傳統 INT4 能更好地保留模型精度。第二是雙重量化（double quantization），對量化常數本身再進行量化，進一步節省記憶體（每個參數平均再省約 0.37 bit）。第三是分頁最佳化器（paged optimizers），利用 NVIDIA 統一記憶體機制，在 GPU 記憶體不足時自動將最佳化器狀態暫存至 CPU 記憶體，避免 OOM 錯誤。

## 記憶體節省幅度

以微調一個 65B 模型為例，全量微調需要超過 780GB 的 GPU 記憶體，即使使用標準 LoRA（FP16）也需約 130GB。QLoRA 透過將基礎模型量化至 4-bit 並僅以 LoRA 更新少量參數，將記憶體需求壓縮至約 48GB，使得單張 A100 或 A6000 即可完成訓練。這對於學術機構與中小型團隊而言是革命性的改變。

## 與標準 LoRA 的效能對比

QLoRA 的論文顯示，在多數基準測試中，QLoRA 微調的模型效能與標準 16-bit LoRA 微調的結果幾乎無差異。Guanaco 模型（QLoRA 微調 LLaMA）在 Vicuna benchmark 上達到了 ChatGPT 效能的 99.3%，證明了大幅壓縮精度並不必然犧牲模型能力。

## 實務考量

QLoRA 的訓練速度通常比標準 LoRA 慢約 30-50%，因為需要在前向與反向傳播過程中進行量化與反量化運算。然而，記憶體的大幅節省使得開發者可以選擇更大的 batch size 或微調更大的模型，在整體效率上仍然極具優勢。實務上需注意量化格式的選擇（NF4 通常優於 FP4）以及學習率的調整。

## Related

- [[lora]]
- [[quantization]]
- [[fine-tuning]]
- [[peft]]
