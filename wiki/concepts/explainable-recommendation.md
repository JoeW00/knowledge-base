---
title: 可解釋推薦 (Explainable Recommendation)
tags: [recommender-system, explainability, llm, reasoning]
created: 2026-04-09
updated: 2026-04-09
---

# 可解釋推薦 (Explainable Recommendation)

可解釋推薦旨在為推薦結果提供使用者可理解的解釋——不僅告訴使用者「你可能喜歡這個」，還說明「為什麼」。這對建立使用者信任、提升推薦透明度、以及滿足監管合規需求至關重要。

## 傳統方法的局限

早期的可解釋推薦多採用模板式方法（「因為你買過 X，所以推薦 Y」）或基於特徵重要性的事後解釋。這些方法的解釋品質有限——模板缺乏個人化與語境適應性，特徵重要性難以轉化為使用者能理解的自然語言。

## LLM 驅動的解釋生成

生成式模型根本性地改變了可解釋推薦的方法。LLM 能夠：(1) 生成流暢的自然語言解釋，針對每個使用者與推薦物品量身定製；(2) 進行多步推理，從使用者歷史行為中推導出偏好邏輯鏈；(3) 整合外部知識（如產品評論、新聞報導）來豐富解釋的深度。

論文將方法分為三類。**顯式推理方法**（如 ReasonRec、Reason-to-Recommend）讓 LLM 先展開推理鏈再產出推薦，類似 [[chain-of-thought]] 的應用。**基於提示的解釋**（如 LLMZER、Graph-enhanced Explanation）利用知識圖譜與檢索增強來提供解釋的事實基礎。**推薦推理**（Recommendation Reasoning）則專注於讓模型具備類似 o1 的慢思考能力，透過監督微調與 RL 最佳化讓推薦系統能進行多步推理並自我修正。

## 核心挑戰

解釋的「忠實性」（faithfulness）是最大的挑戰——生成的解釋是否真正反映了模型的推薦依據，還是只是事後合理化的「幻覺」？這與 [[hallucination]] 問題直接相關。此外，如何在解釋的詳細程度、使用者認知負擔、與推薦效率之間取得平衡，也是開放問題。

## Related

- [[generative-recommendation]]
- [[conversational-recommendation]]
- [[chain-of-thought]]
- [[hallucination]]
- [[generative-recommendation-survey-summary]]
