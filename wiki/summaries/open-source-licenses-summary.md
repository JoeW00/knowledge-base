---
title: 開源授權快速參考摘要
source: raw/articles/Open_Source_Licenses_Guide.md
tags: [licensing, open-source, ai-model, reference]
created: 2026-04-09
updated: 2026-04-09
---

# 開源授權快速參考摘要

一份從最寬鬆到最嚴格排列的開源授權速查表，特別涵蓋 AI 模型圈常見的特殊授權。

傳統開源授權依寬鬆度排列：MIT（最寬鬆）→ Apache 2.0 → BSD → LGPL → MPL 2.0 → GPL → AGPL（最嚴格）。關鍵分界在於是否要求衍生作品也必須開源——MIT/Apache/BSD 不要求，GPL/AGPL 要求。

AI 模型的授權生態更複雜：LLaMA 3 Community License（月活超 7 億需另申請）、Gemma Terms of Use（禁止違法內容生成）、DeepSeek License（有使用限制條款）、CC BY-NC 4.0（禁止商用）。這些都不是傳統意義上的「開源」，而是附帶條件的免費使用授權。

## 快速決策規則

- 商用且不想開源自己的程式碼 → MIT / Apache 2.0 / BSD
- 看到 GPL/AGPL → 整個專案可能須開源
- 看到 NC → 禁止商用
- 大公司自訂授權 → 仔細讀條款，通常有用量或用途限制

## 原文路徑

`raw/articles/Open_Source_Licenses_Guide.md`

## Related

- [[local-llm-inference]]
- [[foundation-model]]
