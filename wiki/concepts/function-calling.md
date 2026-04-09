---
title: Function Calling
tags: [agent, api, llm]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Function calling 是大型語言模型透過 API 呼叫外部工具的核心能力。需要特別注意的是，模型本身並不直接執行工具——它生成結構化的工具呼叫請求（通常為 JSON 格式），由應用層負責實際執行並將結果回傳給模型。這種設計將「決策」（模型決定呼叫什麼工具、傳入什麼參數）與「執行」（應用層實際執行工具）明確分離，既保持了模型的靈活性，也為安全控制提供了介入點。

## JSON Schema 描述工具介面

開發者透過 JSON schema 向模型描述可用工具的介面，包括函式名稱、功能描述、參數定義（名稱、型別、是否必填、預設值）以及回傳值格式。模型根據這些描述判斷何時應呼叫工具以及如何組裝參數。工具描述的品質直接影響模型的呼叫準確率——清晰、完整且具有範例的描述能顯著降低錯誤呼叫的發生。這與 [[prompt-engineering]] 的原則一致：給模型越明確的資訊，它的表現就越好。

## 平行工具呼叫

現代 API 支援模型在單次回應中生成多個工具呼叫，這些呼叫可以平行執行以降低延遲。例如，當使用者問「台北和東京今天的天氣如何？」時，模型可以同時呼叫兩次天氣 API，而非依序執行。平行呼叫在 [[ai-agent]] 系統中尤為重要，能有效加速多步驟任務的完成時間。然而，當工具呼叫之間存在依賴關係時（後一個呼叫需要前一個的結果），就必須依序執行。

## 與 Agent 架構的關係

Function calling 是 [[ai-agent]] 架構的基石。沒有 function calling，agent 就只能生成文字而無法與外部世界互動。在 [[react-framework]] 的 Thought-Action-Observation 迴圈中，Action 階段就是透過 function calling 實現的。Agent 的能力邊界本質上由其可呼叫的工具集合決定——工具庫越豐富且描述越精確，agent 能處理的任務範圍就越廣。

## 安全考量

Function calling 引入了重大的安全風險。模型可能因 [[prompt-injection]] 被誘導呼叫不當的工具或傳入惡意參數。防禦措施包括：實施最小權限原則（僅開放必要的工具）、在 sandbox 環境中執行工具呼叫、對高風險操作要求人類確認、以及對工具參數進行輸入驗證。寫入型工具（如資料庫修改、發送訊息）需要比唯讀型工具更嚴格的安全控制。

## 主要 API 的實作差異

不同模型供應商的 function calling API 存在實作差異。OpenAI 使用 `tools` 參數搭配 `tool_choice` 控制呼叫行為；Anthropic 透過 `tools` 參數提供工具定義並在回應中以 `tool_use` content block 表示呼叫請求。開發者在建構跨模型相容的應用時需要處理這些差異。

## Related

- [[ai-agent]]
- [[react-framework]]
- [[prompt-injection]]
- [[prompt-engineering]]
