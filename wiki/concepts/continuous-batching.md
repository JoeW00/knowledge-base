---
title: "Continuous Batching"
tags: [inference, serving, optimization]
created: 2026-04-09
updated: 2026-04-09
---

## Continuous Batching

Continuous Batching（連續批處理）是一種推理服務排程策略，允許在每個 iteration 層級動態管理批次中的請求，讓完成的請求立即返回並即時插入新請求，從而大幅提升 GPU 利用率與吞吐量。

### 與靜態批處理的對比

傳統的靜態批處理（static batching）會將一組請求打包成固定批次，所有請求必須等到批次中最長的請求完成後才能一起返回。這導致已完成的請求需要無意義地等待，GPU 在等待期間空轉，浪費大量計算資源。例如一個批次中有 8 個請求，最短的可能只需生成 20 個 token，但必須等待最長的 500 個 token 完成後才能返回結果。

### 與動態批處理的差異

動態批處理（dynamic batching）在靜態批處理的基礎上做了改良，允許在批次之間動態調整批次大小與組成。但它仍然以「完整批次」為單位進行排程，無法在批次執行過程中進行調整。Continuous batching 的突破在於將排程粒度從批次層級降至 **iteration 層級**，每一步解碼完成後都可以重新評估批次組成。

### Iteration-level Scheduling

在 continuous batching 中，系統在每個 decode iteration 完成後檢查批次狀態：若某個請求已生成完整回應（遇到終止 token 或達到最大長度），立即將其移出批次並返回結果，同時從等待隊列中取出新請求填入空位。這確保 GPU 始終處於接近滿載的工作狀態，最大化硬體利用效率。

### 效能改善

實務證明，continuous batching 能將推理系統的吞吐量提升數倍，同時降低平均延遲。特別是在請求輸出長度差異大的場景中，改善效果最為顯著。vLLM、TensorRT-LLM、TGI（Text Generation Inference）等主流推理框架均已內建 continuous batching 支援，搭配 PagedAttention 等 [[kv-cache]] 管理技術，構成現代高效推理服務的基礎。

## Related

- [[inference-optimization]]
- [[kv-cache]]
- [[speculative-decoding]]
- [[prompt-caching]]
