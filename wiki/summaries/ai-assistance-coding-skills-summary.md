---
title: AI 輔助如何影響程式技能養成 — 研究摘要
source: raw/articles/How AI assistance impacts the formation of coding skills.md
tags: [ai-economics, human-ai-interaction, skill-development, coding, anthropic]
created: 2026-04-09
updated: 2026-04-09
---

# AI 輔助如何影響程式技能養成 — 研究摘要

Anthropic 的 Judy Hanwen Shen 與 Alex Tamkin 以隨機對照試驗（RCT）探討 AI 輔助對軟體工程師技能學習的影響。52 名初中階工程師被隨機分為 AI 組與手寫組，學習使用陌生的 Python 函式庫 Trio 完成非同步程式設計任務，隨後接受測驗。

**核心發現**：AI 組完成速度僅略快（約 2 分鐘，未達統計顯著），但測驗分數顯著低於手寫組——平均 50% vs. 67%，相當於近兩個等第的差距（Cohen's d=0.738, p=0.01）。差距在 debugging 題型上最大，顯示 AI 輔助可能特別削弱識別與診斷錯誤的能力。

## 關鍵論點

1. **[[cognitive-offloading]] 是技能損耗的核心機制**：重度依賴 AI 生成程式碼的參與者（AI delegation、progressive AI reliance）得分低於 40%，因為他們跳過了獨立除錯與理解的認知過程。
2. **使用方式決定學習效果**：高分組的三種互動模式——「先生成再理解」、「混合程式碼與解釋」、「僅問概念問題」——共同特徵是主動投入認知努力，而非被動接受 AI 輸出。
3. **生產力與技能養成可能存在張力**：AI 可加速已有技能的任務（先前研究顯示最高 80% 提速），但在學習新技能時可能反而阻礙深度理解。
4. **對 AI 產品設計的啟示**：學習模式（如 Claude Code Learning mode）應成為 AI 工具的標準功能，幫助使用者在效率與技能發展之間取得平衡。

## 原文路徑

`raw/articles/How AI assistance impacts the formation of coding skills.md`
（原文來源：[Anthropic Research](https://www.anthropic.com/research/AI-assistance-coding-skills)）

## Related

- [[cognitive-offloading]]
- [[ai-productivity-skill-tradeoff]]
- [[learning-by-doing]]
- [[ai-augmentation-vs-automation]]
- [[skill-biased-technological-change]]
