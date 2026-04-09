---
title: "第03章：評估方法論 摘要"
source: "raw/articles/ai_engineering_chapters/02-評估與品質/第03章-評估方法論.md"
tags: [ai-engineering, evaluation, methodology]
created: 2026-04-09
updated: 2026-04-09
---

## 摘要

本章探討基礎模型評估的核心挑戰與方法論。由於基礎模型具備開放式生成能力、黑盒特性及 benchmark 快速飽和等問題，傳統評估方式已不敷使用。從語言建模指標（交叉熵、困惑度）出發，依序介紹精確評估（功能正確性、詞彙/語義相似度）、LLM-as-judge 主觀評估，以及基於比較的排名方法（Elo、Bradley-Terry）。

## 重點整理

1. **評估是 AI 應用落地的最大瓶頸**——許多團隊仍依賴「憑感覺檢查」，缺乏系統化的評估框架，導致模型品質無法穩定衡量。
2. **精確評估與主觀評估應互補使用**——精確評估適合有明確正確答案的場景，主觀評估則適用於開放式生成任務。
3. **[[llm-as-judge]] 強大但存在偏差**——包括自我偏差（偏好自身生成的回答）、首位偏差（偏好排在前面的選項）、冗長偏差（偏好較長的回答）等系統性問題。
4. **[[comparative-evaluation]] 是模型排名的有力工具**——透過兩兩對比而非絕對評分，能更穩定地反映模型之間的相對能力差異。
5. **[[perplexity]] 的延伸應用廣泛**——除了衡量語言建模品質外，困惑度還可用於資料汙染檢測、異常文字偵測與訓練資料去重。

## Related

- [[perplexity]]
- [[llm-as-judge]]
- [[lexical-semantic-similarity]]
- [[comparative-evaluation]]
- [[functional-correctness]]
- [[embedding]]
