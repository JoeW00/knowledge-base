---
title: "第07章：微調 摘要"
source: "raw/articles/ai_engineering_chapters/04-模型訓練與資料/第07章-微調.md"
tags: [ai-engineering, fine-tuning, training]
created: 2026-04-09
updated: 2026-04-09
---

## 摘要

本章系統介紹 fine-tuning 的完整生命週期。Fine-tuning 透過調整模型權重使其適配特定任務，是遷移學習的核心實作方式。首先探討何時該 fine-tune、何時該用 RAG（RAG 關注事實，fine-tuning 關注形式），接著分析記憶體瓶頸。重點介紹 PEFT 方法，尤其是 LoRA 與 QLoRA。最後討論模型合併技術（求和、層堆疊、拼接）。

## 重點整理

1. 先嘗試 prompt engineering 再考慮 fine-tuning
2. RAG 解決資訊缺失，fine-tuning 解決行為問題，兩者互補
3. LoRA 僅需全量微調 0.01% 的參數即可達到相當效能
4. QLoRA 以 4-bit 格式使單張 48GB GPU 可微調 650 億參數模型
5. 模型合併提供不需額外訓練的模型組合方式

## Related

- [[fine-tuning]]
- [[lora]]
- [[quantization]]
- [[peft]]
- [[model-merging]]
- [[qlora]]
