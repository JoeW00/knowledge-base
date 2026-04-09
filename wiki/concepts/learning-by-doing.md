---
title: Learning-by-Doing（AI 使用的經驗學習效應）
tags: [ai-economics, learning, human-ai-interaction]
created: 2026-04-09
updated: 2026-04-09
---

## Learning-by-Doing

Learning-by-doing（做中學）是一個關鍵的經濟學概念，在 AI 領域指的是使用者透過持續使用 AI 工具而逐漸提升運用效能的現象。Anthropic Economic Index 的 2026 年 3 月報告為這一假說提供了首批大規模實證數據。

### 實證發現

報告分析了 Claude 不同資歷使用者的行為差異，發現高資歷使用者（註冊超過 6 個月）在多個面向表現更佳：

- **成功率**：對話成功率高出 3-5 個百分點。即使控制了任務類型（O*NET 任務固定效應）、模型選擇、語言、國家等變數，高資歷使用者仍有約 4 個百分點的成功率優勢
- **任務複雜度**：每增加一年使用經驗，對應的教育年數需求增加約 1 年
- **工作導向**：使用 Claude 進行工作相關任務的比例高出 7 個百分點
- **互動模式**：更傾向迭代式合作，而非簡單的指令式委託
- **任務多元性**：前 10 大任務佔比更低（20.7% vs. 22.2%），使用面向更廣

### 替代解釋與限制

報告誠實地指出了幾個可能的混淆因素：

1. **自我選擇偏誤**：早期採用者可能本身就是技術能力更強的人（如程式設計師）
2. **存活者偏誤**：長期使用者是那些從 Claude 獲得正面體驗而留下的人，不適合使用的人早已離開
3. **世代效應**：不同註冊時間的使用者群體特質可能本就不同

然而，報告中精心設計的迴歸分析——包括任務固定效應、請求叢集固定效應、以及模型/語言/國家等完整控制變數——排除了這些混淆的簡單版本，顯示經驗效應確實存在。

### 政策與實務意涵

Learning-by-doing 效應意味著 AI 的早期採用不只是時序上的先後，而可能帶來持久的能力優勢。這使得 AI 素養培訓與普及化變得更加緊迫：越晚開始學習，與熟練使用者的差距就越大。對企業而言，這強調了投資員工 AI 技能培訓的重要性——不是一次性的工具教學，而是鼓勵持續使用與探索的文化。

### 認知卸載的風險

然而，Anthropic 的 RCT 研究（[[ai-assistance-coding-skills-summary]]）揭示了 learning-by-doing 的一個重要前提：學習者必須保留認知努力。當 AI 工具讓使用者跳過獨立除錯與理解的過程時，[[cognitive-offloading]] 會抵消做中學的效果。研究顯示，重度依賴 AI 的工程師在測驗中表現顯著較差，但主動使用 AI 來深化理解（而非取代理解）的人仍能有效學習。這意味著 learning-by-doing 的關鍵不在於「做」的速度，而在於「做」的過程中是否投入了足夠的認知努力。

## Related

- [[anthropic-economic-index]]
- [[ai-adoption-curve]]
- [[skill-biased-technological-change]]
- [[ai-augmentation-vs-automation]]
- [[anthropic-economic-index-learning-curves-summary]]
- [[cognitive-offloading]]
- [[ai-productivity-skill-tradeoff]]
- [[ai-assistance-coding-skills-summary]]
