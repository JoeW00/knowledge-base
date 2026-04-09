---
title: 大型推薦模型 (Large Recommendation Model, LRM)
tags: [recommender-system, scaling-law, architecture, generative-ai]
created: 2026-04-09
updated: 2026-04-09
---

# 大型推薦模型 (Large Recommendation Model, LRM)

大型推薦模型（LRM）是一類專為使用者行為資料設計的大規模生成式架構，不直接借用語言模型，而是從頭建構適合推薦場景的模型結構與訓練方法。LRM 的核心主張是：推薦領域需要自己的 scaling law，而非僅將語言模型的 scaling law 遷移過來。

## HSTU：驗證推薦的 Scaling Law

Meta 提出的 HSTU（Hierarchical Sequential Transduction Unit）是 LRM 的奠基之作。它將傳統判別式 CTR 預測任務轉化為生成式序列建模——對每個使用者，將行為序列（互動歷史、使用者屬性、物品特徵）統一為單一序列，以自迴歸方式預測下一個候選物品的機率分布。HSTU 支援 1024 到 8192 的序列長度，參數規模達 1.5 兆，驗證了「模型越大、序列越長、效果越好」的 scaling 趨勢——而傳統判別式推薦模型在約 2000 億參數時就停滯不前。

## 兩個發展方向

工業界的 LRM 研究沿兩條路線推進。第一條是**克服模型複雜度的遞減報酬**：傳統級聯架構（召回→粗排→精排→重排）中，越複雜的判別式模型帶來的邊際改善越小，LRM 透過生成式訓練範式從根本上重新定義模型如何從資料中學習。第二條是**端到端推薦**：LRM 繞過傳統多階段級聯流水線，以單一模型完成從檢索到排序的全流程，降低系統複雜度與階段間的資訊損失。

基於 HSTU，美團提出 MTGR 將判別式交叉特徵融入生成式架構；Redbook 的 OneRec 則以端到端方式統一檢索與排序任務。

## 與 LLM-based 推薦的差異

LLM-based 推薦將使用者行為轉換為自然語言序列再餵入語言模型，優勢在於零樣本泛化與世界知識，但面臨語言建模目標與推薦目標的不匹配。LRM 則直接在原生行為資料上訓練，避免了文本化帶來的資訊損失，更適合大規模工業部署場景。

## Related

- [[generative-recommendation]]
- [[scaling-law]]
- [[foundation-model]]
- [[generative-recommendation-survey-summary]]
