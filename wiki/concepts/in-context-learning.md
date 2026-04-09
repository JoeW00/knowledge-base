---
title: In-Context Learning
tags: [prompt-engineering, learning, llm]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

In-context learning（ICL）是指大型語言模型僅憑提示詞中提供的範例，就能學習並執行新任務的能力。模型不需要額外的參數更新或訓練，僅透過上下文中的示範就能掌握任務模式。這項能力最早由 GPT-3 論文（Brown et al., 2020）系統性揭示，被視為大型語言模型最重要的湧現能力之一。

## Zero-shot vs. Few-shot Learning

根據提供範例的數量，ICL 可分為不同層級。Zero-shot learning 不提供任何範例，僅靠任務描述引導模型完成工作，適用於模型已充分理解的通用任務。Few-shot learning 則在提示詞中加入少量範例（通常二至五個），讓模型從中歸納出輸入與輸出的對應模式。實務上，few-shot 在多數任務中的表現明顯優於 zero-shot，尤其是格式要求嚴格或任務定義較為特殊的場景。

## 範例選擇策略

範例的品質直接影響 ICL 的效果。選擇策略需在相似性與多樣性之間取得平衡——範例應與目標任務足夠相似以具備參考價值，同時應涵蓋不同情境以提升模型的泛化能力。此外，範例的順序也會影響結果。研究顯示，將與目標任務最相似的範例放在最後（最接近模型即將生成的位置）通常能獲得較好的效果。負面範例（展示「不應該」的輸出）有時也能有效引導模型避開常見錯誤。

## 與 Fine-tuning 的取捨

ICL 與 fine-tuning 各有優勢。ICL 不需要訓練資料蒐集與模型訓練的前期投入，能即時調整且適用於快速原型開發。然而，ICL 受限於上下文窗口長度，能放入的範例數量有限，且每次推理都需消耗額外的 token。當任務有大量標註資料且需要穩定的高品質輸出時，fine-tuning 通常是更合適的選擇。在實務中，許多團隊會先以 ICL 進行概念驗證，確認可行後再評估是否需要 fine-tuning。

## Related

- [[prompt-engineering]]
- [[chain-of-thought]]
- [[system-prompt]]
- [[rag]]
