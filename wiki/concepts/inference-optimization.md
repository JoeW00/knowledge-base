---
title: "推理最佳化"
tags: [inference, optimization, deployment]
created: 2026-04-09
updated: 2026-04-09
---

## 推理最佳化

推理最佳化（Inference Optimization）是指在 AI 模型部署後，針對推理階段進行效能調校的一系列技術與策略。與訓練階段不同，推理直接面向終端使用者，因此延遲與成本是最核心的考量。

### 三大最佳化層面

推理最佳化可從三個層面切入。**模型層**聚焦於壓縮與加速模型本身，包含 [[quantization]]、distillation、pruning 以及 [[speculative-decoding]] 等技術。**硬體層**關注計算資源的有效利用，例如選擇適當的 GPU、使用混合精度運算、以及 kernel fusion 等底層最佳化。**服務層**則處理多請求的調度與資源共享問題，涵蓋 [[continuous-batching]]、[[prompt-caching]]、prefill-decode 解耦等策略。

### 關鍵效能指標

衡量推理效能需要一套完整的指標體系。**延遲指標**包含 TTFT（Time To First Token，首 token 生成時間）與 TPOT（Time Per Output Token，每個輸出 token 耗時），前者影響使用者感知的回應速度，後者決定完整回應的生成時間。**吞吐量**以每秒生成的 token 數衡量系統整體處理能力。**硬體效率指標**包含 MFU（Model FLOPS Utilization）與 MBU（Model Bandwidth Utilization），分別衡量計算資源與記憶體頻寬的利用率。

### Compute-bound 與 Memory Bandwidth-bound

推理過程中的瓶頸可分為兩類。**Compute-bound** 出現在 prefill 階段，此時需要處理完整的輸入序列，大量矩陣運算使得計算成為瓶頸。**Memory bandwidth-bound** 出現在 decode 階段，每次只生成一個 token，計算量相對較小，但需要頻繁從記憶體讀取模型權重與 [[kv-cache]]，記憶體頻寬成為限制因素。辨識瓶頸類型是選擇最佳化策略的前提。

### 自託管 vs. API 使用者

對於自託管模型的團隊，三個層面的最佳化都需要考慮，尤其是硬體層與服務層的調校空間最大。對於使用 API 的開發者，則應聚焦於 [[prompt-caching]]、請求批次化、[[model-routing]] 等策略來降低成本與延遲。

## Related

- [[quantization]]
- [[kv-cache]]
- [[speculative-decoding]]
- [[continuous-batching]]
- [[prompt-caching]]
- [[model-routing]]
