---
title: "第09章：推理最佳化 摘要"
source: "raw/articles/ai_engineering_chapters/05-部署與架構/第09章-推理最佳化.md"
tags: [ai-engineering, inference, optimization]
created: 2026-04-09
updated: 2026-04-09
---

## 摘要

本章系統性探討 AI 模型推理最佳化的三大層面：模型、硬體與服務。首先建立推理效能指標體系（延遲、TTFT、TPOT、吞吐量、MFU、MBU），並解析 compute-bound 與 memory bandwidth-bound 兩類瓶頸。模型層面涵蓋 [[quantization]]、distillation、pruning 等壓縮技術，以及 [[speculative-decoding]] 、parallel decoding 等突破自迴歸解碼的方法。服務層面介紹 batching 策略、prefill-decode 解耦、[[prompt-caching]] 與 parallelism 策略。

## 關鍵要點

1. 推理瓶頸分兩類：compute-bound（prefill 階段）與 memory bandwidth-bound（decode 階段）
2. Quantization 是最廣泛適用的模型壓縮技術
3. [[kv-cache]] 是長上下文推理的核心瓶頸
4. [[continuous-batching]]（連續批處理）顯著優於靜態/動態批處理
5. [[prompt-caching]] 對重複性高的工作負載可節省最高 90% 成本

## Related

- [[inference-optimization]]
- [[quantization]]
- [[kv-cache]]
- [[speculative-decoding]]
- [[continuous-batching]]
- [[prompt-caching]]
