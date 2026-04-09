---
title: ReAct Framework
tags: [agent, reasoning, framework]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

ReAct（Reasoning + Acting）是由 Yao et al.（2022）提出的智慧體範式，其核心理念是讓模型在推理（Reasoning）與行動（Acting）之間交替進行。不同於純粹的 [[chain-of-thought]]（只思考不行動）或純粹的工具呼叫（只行動不思考），ReAct 將兩者結合為一個統一的迴圈，使 agent 能夠在每一步都先思考再行動，再根據行動結果調整下一步的思路。

## Thought-Action-Observation 迴圈

ReAct 的運作基於三個階段的循環。Thought 階段，模型以自然語言表達當前的推理過程——分析問題、評估已知資訊、規劃下一步行動。Action 階段，模型透過 [[function-calling]] 呼叫外部工具執行具體操作。Observation 階段，模型接收工具回傳的結果，作為下一輪 Thought 的輸入。這個迴圈持續進行，直到模型判斷已收集足夠資訊可以給出最終答案，或達到預設的最大迭代次數。

## 與純 CoT 和純 Action 的比較

純 CoT 方法讓模型只靠內部知識進行推理，無法存取外部資訊或執行動作，容易在需要即時資料或複雜計算的任務中失敗。純 Action 方法讓模型直接呼叫工具而不展開推理過程，容易做出缺乏策略的盲目行動。ReAct 結合兩者的優勢：推理為行動提供方向，行動為推理提供新資訊。實驗證明，在需要多步資訊蒐集與推理的任務（如多跳問答、事實驗證）中，ReAct 顯著優於單獨使用任一方法。

## Reflexion 反思框架

Reflexion 是 ReAct 的重要延伸，引入「從失敗中學習」的機制。當 agent 完成一次嘗試後，無論成功或失敗，Reflexion 都會讓模型回顧整個過程，生成反思摘要（reflection），指出哪些步驟做得好、哪些需要改進。這些反思會被保存在記憶中，供後續嘗試參考。透過反覆試錯與反思，agent 能逐步改善解題策略，在需要多次嘗試的複雜任務中表現出色。這與 [[memory-management]] 的長期記憶概念緊密相關。

## 實作考量

在實際部署 ReAct agent 時，有幾個關鍵的工程考量。停止條件的設計至關重要——agent 需要知道何時停止迴圈並給出答案。最大迭代次數是最基本的安全閥，防止 agent 陷入無限迴圈。錯誤處理機制需要能優雅地應對工具呼叫失敗、回傳格式異常等情況。此外，每一輪迴圈都會消耗 token，因此需要在推理品質與成本之間取得平衡。

## Related

- [[chain-of-thought]]
- [[ai-agent]]
- [[function-calling]]
- [[memory-management]]
