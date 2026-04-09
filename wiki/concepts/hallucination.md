---
title: Hallucination
tags: [safety, evaluation, foundation-model]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Hallucination（幻覺）是指 [[foundation-model]] 生成看似合理但缺乏事實依據的內容。這是當前 AI 應用最棘手的挑戰之一，因為幻覺內容往往文法流暢、邏輯自洽，使用者難以僅憑直覺辨別真偽。在醫療、法律、金融等高風險領域，幻覺可能導致嚴重後果。

## 兩種假說

關於幻覺的成因，學界存在兩種主要假說。第一種是「自我欺騙假說」，認為模型在訓練過程中學會了過度自信地生成回應，即使缺乏足夠的知識支撐也傾向給出看似確定的答案，而非承認不確定性。第二種是「知識不匹配假說」，認為模型的參數化知識與真實世界知識之間存在落差——模型可能記住了錯誤的資訊，或在不同知識片段之間建立了錯誤的關聯，導致生成時將不相關的資訊拼接在一起。

## Intrinsic vs. Extrinsic Hallucination

幻覺可進一步區分為兩種類型。Intrinsic hallucination（內在幻覺）是指模型生成與輸入資料直接矛盾的內容，例如在摘要任務中捏造原文未提及的事實。Extrinsic hallucination（外在幻覺）則是指模型生成無法從輸入資料驗證的內容——這些內容可能正確也可能錯誤，但關鍵在於缺乏依據。在實務應用中，intrinsic hallucination 通常更容易被檢測，因為有明確的參照文本可供比對。

## 與 Sampling 機率特性的關係

幻覺與 [[sampling-strategy]] 的機率特性密切相關。模型在每個步驟選擇的 token 都帶有不確定性，當多個低確信度的選擇累積時，生成內容就可能偏離事實。較高的 temperature 設定會放大這種效應，增加幻覺發生的機率。然而，即使使用 greedy decoding（溫度為零），模型仍可能產生幻覺，這表明問題的根源不僅在於取樣過程，更在於模型內部的知識表徵本身。

## 檢測方法

檢測幻覺的核心方法是 factual consistency 驗證。常見做法包括：將模型輸出與可信來源進行事實比對；使用自然語言推理（NLI）模型判斷生成內容是否與參考文本一致；讓模型多次回答同一問題，透過回應的一致性程度來估計不確定性；以及使用專門訓練的幻覺檢測模型進行自動化評估。

## 緩解策略

目前已有多種緩解幻覺的策略。RAG（Retrieval-Augmented Generation）透過引入外部知識庫的即時資訊，為模型提供可靠的事實基礎，是最廣泛採用的方案。Chain-of-thought prompting 引導模型逐步推理，使推理過程透明化並減少跳躍性錯誤。事實驗證機制在模型輸出後進行自動化核查，攔截明顯的幻覺內容。此外，透過 [[rlhf]] 等後訓練方法，也可以教導模型在不確定時表達不確定性，而非捏造答案。

## Related

- [[sampling-strategy]]
- [[foundation-model]]
- [[rlhf]]
- [[rag]]
- [[factual-consistency]]
- [[chain-of-thought]]
- [[guardrails]]
- [[ch02-understanding-foundation-models-summary]]
