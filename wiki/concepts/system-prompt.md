---
title: System Prompt
tags: [prompt-engineering, architecture, llm]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

System prompt 是位於提示詞起始位置的特殊區塊，用於設定模型的角色、行為邊界與全域規則。它是整個對話的「指揮中心」，決定模型以何種身份、在何種限制條件下回應使用者。與 user prompt 不同，system prompt 通常由開發者預先設定且對終端使用者不可見，在整個對話過程中持續生效。

## 與 User Prompt 的區別與優先級

User prompt 承載使用者的具體任務或問題，而 system prompt 則定義模型處理這些任務的方式。在理想情況下，system prompt 的優先級應高於 user prompt——當兩者存在衝突時，模型應遵循 system prompt 的指令。然而，實務上模型並不總是能可靠地維持這一優先級，這正是 [[prompt-injection]] 攻擊得以奏效的原因之一。不同模型和 API 對 system prompt 優先級的實作也有差異。

## 典型元素

一個設計良好的 system prompt 通常包含以下元素。首先是角色定義，明確告訴模型「你是什麼」，例如「你是一位專業的法律助理」。其次是輸出格式規範，指定回應的結構、長度與語氣。第三是限制規則，定義模型不應該做的事情，例如「不要提供醫療診斷建議」。最後可包含少量範例，示範期望的回應模式。這些元素的組合構成了模型的「行為框架」，確保每次回應都在預設的軌道上運行。

## 安全考量

System prompt 不應包含敏感資訊，因為它可能被 [[prompt-injection]] 中的 prompt extraction 攻擊洩漏。API 金鑰、內部系統資訊、商業機密等絕不應出現在 system prompt 中。此外，system prompt 的內容應假設可能被使用者看到，在設計時就考慮到這一風險。部分服務商提供了強化 system prompt 優先級的機制（如 system message 角色標記），但這些措施也無法完全保證安全。

## 不同 API 的處理方式

各大模型 API 對 system prompt 的處理方式存在差異。OpenAI 的 Chat Completions API 透過 `role: "system"` 訊息實現，而 Anthropic 的 API 則使用獨立的 `system` 參數。部分模型在訓練時對 system prompt 有特殊的注意力機制，使其指令具有較高的遵循度。開發者在跨模型遷移時需注意這些差異，並針對目標模型調整 system prompt 的寫法。

## Related

- [[prompt-engineering]]
- [[prompt-injection]]
- [[in-context-learning]]
- [[prompt-versioning]]
