---
title: Prompt Engineering
tags: [prompt-engineering, ai-engineering, methodology]
created: 2026-04-09
updated: 2026-04-09
---

## 定義與範疇

Prompt engineering 是透過精心設計提示詞來引導大型語言模型產出符合預期結果的工程方法。它不僅僅是「寫出好的提問」，而是一門需要系統性實驗、評估與迭代的嚴謹工程學科。在 AI 應用開發中，prompt engineering 是成本最低、見效最快的最佳化手段，適用於絕大多數場景的第一步調校。

## 提示詞結構

一個完整的提示詞通常由三個核心部分組成。首先是 [[system-prompt]]，用於設定模型的角色、行為邊界與輸出格式；其次是 user prompt，承載使用者的具體任務或問題；最後是 context，提供模型完成任務所需的背景資訊，例如透過 [[rag]] 檢索而來的文件片段。這三者的設計需要相互配合，確保模型能正確理解任務意圖並依循限制規則。

## 核心技巧

有效的 prompt engineering 依賴幾項關鍵技巧。第一是清晰指令——模型無法揣測模糊意圖，因此應明確指定輸出格式、長度、語氣等要求。第二是提供範例，即 [[in-context-learning]]，透過少量範例讓模型快速掌握任務模式。第三是任務拆分，將複雜任務分解為多個子任務，各自配備專屬提示詞，降低單次推理的難度。第四是 [[chain-of-thought]]，要求模型逐步推理以提升正確率並減少幻覺。

## 與 Fine-tuning 和 RAG 的比較

Prompt engineering 是三種模型最佳化策略中最輕量的方式。相較於 fine-tuning 需要準備訓練資料並消耗大量運算資源，prompt engineering 只需調整文字即可生效。然而其上限較低，對於需要大量領域知識或特定行為模式的任務，fine-tuning 仍是必要手段。[[rag]] 則透過外部檢索補充模型知識，與 prompt engineering 互補而非互斥。實務上，三者常結合使用以達到最佳效果。

## 工程化實踐

成熟的 prompt engineering 工作流應包含 [[prompt-versioning]]（版本管理）、A/B 測試、以及 prompt catalog 的建立。提示詞應與程式碼分離儲存，每次修改都需記錄版本並追蹤效能變化。透過 A/B 測試比較不同提示詞版本的表現，並將驗證有效的提示詞納入組織的 prompt catalog，供團隊共享與複用。

## Related

- [[in-context-learning]]
- [[chain-of-thought]]
- [[system-prompt]]
- [[prompt-injection]]
- [[prompt-versioning]]
- [[rag]]
