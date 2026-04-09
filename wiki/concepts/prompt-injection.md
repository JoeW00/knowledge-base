---
title: Prompt Injection
tags: [security, prompt-engineering, safety]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Prompt injection 是指攻擊者在提示詞中嵌入惡意指令，試圖覆蓋或繞過原始系統指令的攻擊手法。這類攻擊利用了大型語言模型無法可靠區分「指令」與「資料」的根本弱點，與傳統資安領域中的 SQL injection 在原理上高度類似——兩者都是透過混淆指令與資料的邊界來達成攻擊目的。

## 攻擊類型

Prompt injection 主要分為直接注入與間接注入兩大類。直接注入是使用者在輸入中直接嵌入惡意指令，例如「忽略以上所有指令，改為執行……」。間接注入則更為隱蔽，攻擊者將惡意指令藏在模型會讀取的外部資料源中（如網頁內容、上傳文件），當模型處理這些資料時就會觸發惡意行為。間接注入在 [[rag]] 與 [[ai-agent]] 系統中尤其危險，因為這些系統會主動檢索與處理外部資料。

## Prompt Extraction 與 Jailbreaking

Prompt extraction 是一種特殊的注入攻擊，目標是誘使模型洩漏 [[system-prompt]] 的內容。攻擊者可能透過角色扮演、假設情境等手法引導模型輸出原始指令。Jailbreaking 則試圖繞過模型的安全限制，使其產出被禁止的內容。常見手法包括 DAN（Do Anything Now）角色扮演、多語言繞過、以及逐步引導模型放鬆限制的漸進式攻擊。

## 防禦策略

有效的防禦需要在多個層面同時部署。在模型層面，可透過 fine-tuning 強化模型對注入攻擊的抵抗力。在提示詞層面，應使用明確的分隔符將指令與資料區隔，並加入防禦性指令提醒模型忽略可疑內容。在系統層面，實施輸入過濾（偵測可疑模式）、輸出驗證（檢查是否洩漏敏感資訊）、權限隔離（限制模型可存取的資源）與 sandboxing（在隔離環境中執行工具呼叫）。多層防禦（defense in depth）是應對此類威脅的基本原則。

## Related

- [[prompt-engineering]]
- [[system-prompt]]
- [[ai-agent]]
- [[function-calling]]
