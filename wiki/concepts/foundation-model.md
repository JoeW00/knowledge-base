---
title: Foundation Model
tags: [foundation-model, ai-engineering, llm]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Foundation model（基礎模型）是指透過大規模自監督預訓練所得到的通用模型，能夠處理文字、影像、音訊等多模態資料，並作為各種下游應用的基礎。與傳統機器學習模型針對單一任務訓練不同，foundation model 具備廣泛的通用能力，可透過少量適配即可應用於多種場景。

## 發展歷程

Foundation model 的演進經歷了三個關鍵階段。首先是語言模型階段，研究者以統計方法建模文字序列的機率分佈。接著，[[self-supervised-learning]] 的突破使語言模型得以在海量無標註文字上訓練，誕生了大語言模型（LLM）。最後，透過將文字、影像、音訊等多模態資料統一為 token 序列，模型進一步演化為真正的多模態 foundation model，能夠同時理解並生成不同類型的內容。

## 三種適配方法

Foundation model 的強大之處在於其靈活的適配能力。第一種方法是 prompt engineering，透過精心設計提示詞引導模型產生期望輸出，無需修改模型參數。第二種是 RAG（Retrieval-Augmented Generation），將外部知識庫的資訊檢索後注入上下文，讓模型能回答訓練資料中未涵蓋的問題。第三種是 fine-tuning，在特定領域資料上進一步訓練模型參數，使其專精於特定任務。這三種方法可以單獨使用，也可以組合運用以達到最佳效果。

## MaaS 商業模式

模型即服務（Model as a Service, MaaS）是 foundation model 催生的新商業模式。OpenAI、Anthropic、Google 等公司提供 API 存取，開發者無需自行訓練或部署模型即可建構 AI 應用。這大幅降低了進入門檻，使 AI 工程成為成長最快的工程領域之一。

## 與傳統 ML 模型的差異

傳統 ML 模型通常針對單一任務、在標註資料上以監督學習方式訓練。Foundation model 則透過自監督學習在大規模未標註資料上預訓練，習得通用表徵後再適配至各任務。這意味著 AI 工程師的重心從模型開發轉移至模型適配與評估，技術棧也因此發生根本性變化。

## Related

- [[self-supervised-learning]]
- [[prompt-engineering]]
- [[rag]]
- [[fine-tuning]]
- [[transformer-architecture]]
- [[scaling-law]]
- [[ch01-ai-app-intro-summary]]
- [[ch02-understanding-foundation-models-summary]]
- [[generative-recommendation]]
- [[generative-recommendation-survey-summary]]
