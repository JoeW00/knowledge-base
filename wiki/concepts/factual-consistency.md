---
title: "事實一致性（Factual Consistency）"
tags: [evaluation, safety, hallucination]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

事實一致性是衡量模型生成內容與事實是否吻合的重要指標，也是對抗幻覺（hallucination）問題的核心評估維度。隨著 LLM 被廣泛應用於摘要、問答、報告生成等場景，確保輸出內容的事實正確性成為 AI 工程中的關鍵挑戰。

## 局部事實一致性

局部事實一致性（local factual consistency）是指模型輸出與給定上下文之間的一致程度。這在 RAG（Retrieval-Augmented Generation）場景中尤為重要：系統從知識庫中檢索到相關文件後，模型應該基於這些文件生成回答，而非憑空捏造。局部驗證的優勢在於有明確的比對對象——只要檢查生成內容中的每個事實宣稱是否都能在提供的上下文中找到依據即可。

## 全局事實一致性

全局事實一致性（global factual consistency）則是對照公開知識或客觀事實來驗證模型輸出。例如判斷「台灣的首都是台北」這類陳述是否正確。全局驗證的困難度遠高於局部驗證，因為需要一個可靠的「事實來源」作為比對基準，而公開知識本身可能存在爭議、過時或不完整等問題。

## NLI 模型驗證法

自然語言推理（Natural Language Inference, NLI）模型是自動化事實一致性驗證的常用工具。NLI 模型判斷兩個句子之間是「蘊含（entailment）」、「矛盾（contradiction）」還是「中立（neutral）」的關係。透過將模型輸出分解為多個事實宣稱，再逐一與來源文本進行 NLI 判斷，可以系統化地檢測不一致之處。

## 事實定義的挑戰

在實務中，定義什麼算「事實」本身就是一個挑戰。某些陳述可能在特定時間點為真但後來過時；有些陳述在不同文化或語境下有不同解讀；還有些涉及推理或推論的陳述，其正確性取決於推理鏈的每一步是否成立。因此，[[evaluation-driven-development]] 中需要為具體應用場景制定清晰的事實一致性定義與評估標準。

## Related

- [[evaluation-driven-development]]
- [[evaluation-pipeline]]
- [[llm-as-judge]]
- [[benchmark-contamination]]
