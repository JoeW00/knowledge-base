---
title: "Guardrails"
tags: [safety, architecture, deployment]
created: 2026-04-09
updated: 2026-04-09
---

## Guardrails

Guardrails（防護機制）是 AI 應用中對模型輸入與輸出實施的安全防護措施，確保系統的回應安全、合規、且符合預期品質標準。

### 輸入防護

輸入端的防護措施在使用者查詢送達模型之前進行攔截與處理。**PII 偵測與脫敏**自動識別並遮蔽個人可識別資訊（姓名、身分證字號、電話號碼等），避免敏感資料進入模型。**毒性過濾**偵測並阻擋包含仇恨言論、騷擾、暴力等有害內容的輸入。**Prompt injection 偵測**識別試圖操控模型行為的惡意提示詞，這是 LLM 應用獨有的攻擊向量。**Topic 限制**確保使用者查詢落在應用預設的主題範圍內，防止模型被引導至不相關或敏感的領域。

### 輸出防護

輸出端的防護措施在模型回應返回使用者之前進行檢查。**事實一致性驗證**比對模型回應與提供的上下文資料是否一致，降低幻覺風險。**格式驗證**確保輸出符合預期的結構化格式，例如 JSON schema 驗證或必要欄位檢查。**敏感內容過濾**攔截模型可能生成的不當內容，即使輸入本身並無問題。

### 延遲影響

每一層防護措施都會增加請求的端到端延遲。PII 偵測、毒性分類、prompt injection 偵測各自可能增加數十到數百毫秒的處理時間。在設計架構時需要權衡安全性與使用者體驗，常見做法包括平行執行多個檢查、對低風險查詢降低檢查強度、以及使用更輕量的模型進行防護判斷。

### 開源框架

**NeMo Guardrails**（由 NVIDIA 開發）提供了一個宣告式的防護規則框架，支援 topical rails、moderation rails、以及自定義對話流程控制。**Guardrails AI** 則聚焦於結構化輸出驗證，提供 validators 套件來檢查輸出是否符合預期。這些框架降低了實作 guardrails 的門檻，但通常需要根據具體應用場景進行客製化調整。

### 與 System Prompt 的互補

System prompt 中的安全指令是第一道防線，告訴模型什麼該做、什麼不該做。但 system prompt 並非萬無一失——模型可能忽略指令，或被精心設計的 prompt injection 繞過。Guardrails 作為獨立於模型的外部檢查機制，提供了更可靠的安全保障。兩者搭配使用，構成深度防禦（defense in depth）策略。

## Related

- [[ai-gateway]]
- [[observability]]
- [[user-feedback-loop]]
- [[model-routing]]
