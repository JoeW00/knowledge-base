---
title: "模型選擇流程（Model Selection Workflow）"
tags: [methodology, ai-engineering, deployment]
created: 2026-04-09
updated: 2026-04-09
---

## 概述

模型選擇流程是一套結構化的四階段方法，幫助團隊在眾多可用的 AI 模型中找到最適合特定應用場景的選擇。隨著模型生態日趨豐富，從開源到閉源、從通用到領域特定，系統化的選擇流程變得不可或缺。

## 第一階段：硬屬性篩選

第一階段透過不可妥協的硬性條件快速縮小候選範圍。常見的篩選條件包括：**語言支援**（模型是否支援目標語言，如繁體中文）、**授權條款**（開源授權是否允許商業使用）、**隱私與合規**（資料是否需要留在本地、是否符合特定法規如 GDPR）、**上下文長度**（是否滿足應用所需的 token 長度）。這個階段的目的是排除明顯不符合需求的選項，通常可以將候選模型從數十個縮減至個位數。

## 第二階段：公開 Benchmark 初篩

通過硬屬性篩選的模型會進入 benchmark 初篩。參考公開排行榜（如 LMSYS Chatbot Arena、Open LLM Leaderboard）和標準 benchmark 的表現，對候選模型的通用能力有初步認識。但必須注意 [[benchmark-contamination]] 的風險——公開 benchmark 的分數可能因資料汙染而失真，因此這個階段的結果僅供參考，不應作為最終決策依據。

## 第三階段：私有評估流水線驗證

這是最關鍵的階段。團隊使用自建的 [[evaluation-pipeline]]，以貼近實際應用場景的測試資料對候選模型進行深入評估。評估維度通常包括：領域知識的準確性、[[instruction-following]] 的可靠度、[[factual-consistency]]、生成品質、以及回應延遲。這個階段的評估資料應該保密，以避免未來被納入模型訓練。

## 第四階段：生產環境監控

選定模型並部署後，需要持續監控其在真實環境中的表現。生產環境中的資料分佈可能與評估集不同，使用者的實際需求也可能超出測試覆蓋範圍。透過收集使用者回饋、追蹤關鍵指標（如滿意度、錯誤率、延遲），確保模型持續符合預期。

## 自建 vs. 購買

模型選擇流程中還需考慮「自建 vs. 購買」的策略決策。使用第三方 API（如 OpenAI、Anthropic）能快速上線但犧牲控制權；自行部署開源模型（如 Llama、Mistral）則有更高的彈性與隱私保障，但需要投入基礎設施與維運成本。最佳選擇取決於團隊的技術能力、預算、資料敏感度與規模需求。

## Related

- [[benchmark-contamination]]
- [[evaluation-pipeline]]
- [[evaluation-driven-development]]
- [[instruction-following]]
- [[comparative-evaluation]]
