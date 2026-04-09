---
title: "第10章：AI 工程架構與使用者反饋 摘要"
source: "raw/articles/ai_engineering_chapters/05-部署與架構/第10章-AI工程架構與使用者反饋.md"
tags: [ai-engineering, architecture, feedback, deployment]
created: 2026-04-09
updated: 2026-04-09
---

## 摘要

本章分為兩大部分。第一部分以漸進式方式建構 AI 應用架構：從最簡查詢-回應模式出發，依序加入上下文增強（RAG、工具呼叫）、輸入/輸出防護措施（PII 脫敏、毒性檢測）、router 與 [[ai-gateway]]、快取層、agent 模式，最後涵蓋監控 [[observability]] 與流水線編排。第二部分探討對話式 AI 的 [[user-feedback-loop]]，包括自然語言回饋提取、隱式回饋訊號設計，以及回饋偏差與惡性回饋循環等風險。

## 關鍵要點

1. AI 應用架構應漸進式演進，從簡單模型呼叫開始逐步加入元件
2. [[ai-gateway]] 是多模型管理的關鍵中間層
3. 使用者回饋是 data flywheel 的核心燃料
4. [[observability]] 不是事後補充而是架構核心
5. 回饋設計須警惕偏差與惡性循環

## Related

- [[ai-gateway]]
- [[model-routing]]
- [[semantic-caching]]
- [[guardrails]]
- [[user-feedback-loop]]
- [[observability]]
