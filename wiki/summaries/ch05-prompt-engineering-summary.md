---
title: "第05章：提示工程 摘要"
source: "raw/articles/ai_engineering_chapters/03-提示工程與RAG/第05章-提示工程.md"
tags: [ai-engineering, prompt-engineering, security]
created: 2026-04-09
updated: 2026-04-09
---

## 摘要

本章系統性介紹 prompt engineering 的原理與最佳實踐。涵蓋提示詞的基本結構（任務描述、範例、具體任務）、in-context learning（零樣本與少樣本學習）、system prompt 與 user prompt 的區別。在最佳實踐方面，強調清晰指令、提供範例、任務拆分、[[chain-of-thought]] 等技巧，並討論提示詞版本管理。後半部分深入探討防禦性 prompt engineering，包括 prompt extraction、jailbreaking、[[prompt-injection]] 等攻擊手法及防禦措施。

## 重點整理

1. [[prompt-engineering]] 是嚴謹的工程學科，應以系統化方式進行實驗與評估
2. [[chain-of-thought]] 能顯著提升推理表現並減少幻覺
3. 複雜任務應拆分為子任務，獨立設計提示詞
4. 提示詞攻擊是重大安全風險，需在模型、提示詞與系統三個層面防禦
5. 提示詞應與程式碼分離並進行版本管理

## Related

- [[prompt-engineering]]
- [[in-context-learning]]
- [[chain-of-thought]]
- [[prompt-injection]]
- [[system-prompt]]
- [[prompt-versioning]]
