---
title: Wiki 索引
updated: 2026-04-09
---

# Knowledge Base Wiki 索引

## 統計資訊

| 項目 | 數量 |
|------|------|
| Concepts | 83 篇 |
| Summaries | 21 篇 |
| Explorations | 2 篇 |
| 總字數（估計） | ~10,600 字 |
| 最近更新 | 2026-04-09 |

---

## Concepts 分類

### Agent Memory 與認知架構
- [[agent-memory-taxonomy]] — Forms–Functions–Dynamics 統一分類框架
- [[token-level-memory]] — 顯式離散單元的 1D/2D/3D 記憶組織
- [[experiential-memory]] — 從成功與失敗軌跡提煉策略與技能的經驗記憶
- [[memory-lifecycle]] — 記憶形成、演化、檢索的動態循環
- [[context-engineering]] — 將上下文窗口視為受限資源的系統性最佳化方法論
- [[ai-memory-system]] — AI 助手跨對話保留與檢索互動資訊的基礎設施
- [[memory-management]] — Agent 系統中短期與長期記憶的管理機制

### AI Agent 與工具使用
- [[ai-agent]] — 能感知環境並透過工具作用於環境的自主系統
- [[react-framework]] — Reasoning + Acting 交替進行的智慧體範式
- [[function-calling]] — LLM 透過 API 呼叫外部工具的核心能力

### 基礎模型與架構
- [[foundation-model]] — 大規模自監督預訓練的通用模型
- [[transformer-architecture]] — 當前幾乎所有基礎模型的核心骨幹
- [[attention-mechanism]] — Transformer 中動態權衡不同 token 重要性的組件
- [[scaling-law]] — 模型規模與效能之間關係的經驗法則
- [[self-supervised-learning]] — 從資料自身結構推導監督信號的訓練方式
- [[tokenization]] — 將原始文字拆分為 token 的前處理步驟

### Prompt Engineering
- [[prompt-engineering]] — 精心設計提示詞來引導模型產出的工程方法
- [[chain-of-thought]] — 要求模型逐步展開推理過程的提示技術
- [[in-context-learning]] — 模型憑提示詞中範例學習新任務的能力
- [[system-prompt]] — 設定模型角色與行為邊界的特殊區塊
- [[prompt-injection]] — 在提示詞中嵌入惡意指令的攻擊手法
- [[prompt-versioning]] — 將提示詞視為工程產物進行版本控制
- [[prompt-caching]] — 快取重複提示詞片段以避免重複計算

### RAG 與檢索
- [[rag]] — 透過外部檢索增強語言模型生成品質的架構模式
- [[vector-search]] — 基於嵌入向量的語義檢索技術
- [[embedding]] — 將非結構化資料轉換為固定維度數值向量
- [[lexical-semantic-similarity]] — 衡量文本相近程度的兩大類方法
- [[semantic-caching]] — 基於語義相似度命中快取的技術

### 評估與品質
- [[evaluation-driven-development]] — 先定義評估標準再建構 AI 應用的方法論
- [[evaluation-pipeline]] — 系統化、可重複執行的 AI 評估流程
- [[llm-as-judge]] — 利用 LLM 評估其他模型輸出品質的方法
- [[comparative-evaluation]] — 模型兩兩對比產生排名的評估方式
- [[functional-correctness]] — 根據系統是否正確實現預期功能的評估
- [[perplexity]] — 衡量語言模型預測不確定程度的核心指標
- [[benchmark-contamination]] — 訓練資料含評估集導致分數虛高的問題
- [[instruction-following]] — 衡量模型能否按指令行事的能力
- [[factual-consistency]] — 模型生成內容與事實吻合程度的指標

### Fine-Tuning 與訓練
- [[fine-tuning]] — 在預訓練模型基礎上進一步調整權重的技術
- [[peft]] — 以少量參數達到接近全量微調效能的技術總稱
- [[lora]] — 將權重更新分解為低秩矩陣的參數高效微調方法
- [[qlora]] — 結合 4-bit 量化與 LoRA 的微調方法
- [[rlhf]] — 基於人類反饋的強化學習後訓練技術
- [[model-distillation]] — 以大模型輸出訓練小模型的知識蒸餾
- [[model-merging]] — 不需額外訓練即合併多模型能力的技術

### 資料工程
- [[data-quality]] — 訓練資料品質決定微調效能上限
- [[data-curation]] — 從蒐集到清理的完整資料策展流程
- [[data-augmentation]] — 基於既有資料生成新訓練樣本的技術
- [[synthetic-data]] — 透過規則或 AI 程式化生成的訓練資料
- [[model-collapse]] — 遞迴使用 AI 生成資料導致效能退化的現象

### 推理與部署
- [[inference-optimization]] — 模型部署後推理階段的效能調校技術
- [[kv-cache]] — Transformer 推理階段儲存 key-value 向量的快取機制
- [[speculative-decoding]] — 用小模型加速大模型推理的技術
- [[continuous-batching]] — 每個 iteration 動態管理批次請求的排程策略
- [[quantization]] — 降低參數數值精度以壓縮模型的技術
- [[sampling-strategy]] — 從機率分佈選取下一個 token 的方法
- [[model-routing]] — 依查詢特性動態分配至不同模型的策略
- [[model-selection-workflow]] — 四階段結構化模型選擇方法
- [[ai-model-selection]] — 使用者在不同模型間做選擇的決策過程

### 系統架構與安全
- [[ai-gateway]] — 應用與多個 AI 提供商之間的中間層
- [[guardrails]] — 對模型輸入與輸出實施的安全防護措施
- [[hallucination]] — 模型生成看似合理但缺乏事實依據的內容
- [[observability]] — 透過 metrics、logs、traces 監控 AI 系統的能力
- [[user-feedback-loop]] — 從 AI 互動中系統性提取回饋訊號的閉環機制

### 本地推理
- [[local-llm-inference]] — 在個人電腦運行 LLM 完整推理流程
- [[ollama]] — 一站式本地模型管理工具（CLI + REST API）
- [[mlx]] — Apple 為 Apple Silicon 設計的機器學習框架

### 深度學習基礎
- [[backpropagation]] — 訓練神經網路的核心演算法
- [[activation-function]] — 人工神經元的非線性轉換函數
- [[convolutional-neural-network]] — 從空間結構資料提取特徵的 CNN 架構
- [[recurrent-neural-network]] — 處理序列資料的 RNN 架構
- [[autoencoder]] — 學習壓縮與重建輸入的神經網路架構
- [[generative-adversarial-network]] — 兩個網路互相對抗訓練的 GAN 架構
- [[reinforcement-learning]] — 透過環境獎勵信號學習最佳策略的訓練範式
- [[overfitting-underfitting]] — 記住細節 vs. 學到通用規則的平衡兩難

### 推薦系統
- [[generative-recommendation]] — 將推薦從排序問題重定義為生成問題的新範式
- [[large-recommendation-model]] — 專為使用者行為資料設計的大規模生成式架構
- [[diffusion-recommendation]] — 擴散模型去噪過程應用於推薦系統
- [[conversational-recommendation]] — 透過多輪對話理解需求並提供推薦
- [[explainable-recommendation]] — 為推薦結果提供可理解解釋

### AI 經濟與人機互動
- [[anthropic-economic-index]] — Anthropic 追蹤 Claude 經濟使用狀況的研究框架
- [[ai-adoption-curve]] — AI 技術從早期採用者擴散至大眾的過程
- [[skill-biased-technological-change]] — 技術創新不成比例地提升高技能勞工生產力
- [[cognitive-offloading]] — 將心智任務委託給外部工具的現象
- [[ai-productivity-skill-tradeoff]] — AI 加速執行但可能阻礙深度學習的張力
- [[learning-by-doing]] — 持續使用 AI 工具逐漸提升運用效能的經驗學習
- [[ai-augmentation-vs-automation]] — AI 對工作影響的增強 vs. 自動化兩條路線

---

## Summaries

### AI Engineering（Chip Huyen）
- [[ch01-ai-app-intro-summary]] — 第 1 章：基於基礎模型構建 AI 應用入門
- [[ch02-understanding-foundation-models-summary]] — 第 2 章：理解基礎模型
- [[ch03-evaluation-methodology-summary]] — 第 3 章：評估方法論
- [[ch04-evaluating-ai-systems-summary]] — 第 4 章：評估 AI 系統
- [[ch05-prompt-engineering-summary]] — 第 5 章：提示工程
- [[ch06-rag-and-agents-summary]] — 第 6 章：RAG 與智慧體
- [[ch07-fine-tuning-summary]] — 第 7 章：微調
- [[ch08-dataset-engineering-summary]] — 第 8 章：資料集工程
- [[ch09-inference-optimization-summary]] — 第 9 章：推理最佳化
- [[ch10-ai-architecture-feedback-summary]] — 第 10 章：AI 工程架構與使用者反饋

### Deep Learning：從基礎到實作
- [[deep-learning-from-basic-to-implementation-summary]] — 全書摘要（29 章）

### MemPalace 記憶系統
- [[mempalace-report-summary]] — MemPalace AI 記憶系統報告摘要
- [[mempalace-benchmarks-summary]] — LongMemEval 基準測試結果
- [[mempalace-integration-summary]] — 整合範例與工作流
- [[mempalace-full-manual-summary]] — 完整教學手冊

### 研究論文
- [[agent-memory-survey-summary]] — Memory in the Age of AI Agents 綜述
- [[generative-recommendation-survey-summary]] — 生成式推薦系統綜述
- [[anthropic-economic-index-learning-curves-summary]] — Anthropic 經濟指數：學習曲線報告
- [[ai-assistance-coding-skills-summary]] — AI 輔助對程式技能養成的影響

### 指南與工具
- [[local-llm-guides-summary]] — 本地 LLM 推理系列指南（4 篇）
- [[open-source-licenses-summary]] — 開源授權指南

---

## Explorations

- [[rag-accuracy-strategies]] — RAG 準確率提升策略探索
- [[chinchilla-scaling-tokens-parameters]] — Chinchilla 縮放定律：token 與參數的最佳比例

---

## 最近新增（前 10 篇）

所有文章均建立於 2026-04-09（知識庫初始建置日）。最近一批新增：

1. [[agent-memory-taxonomy]] — Agent Memory Forms–Functions–Dynamics 框架
2. [[token-level-memory]] — Token 層級記憶的 1D/2D/3D 拓撲
3. [[experiential-memory]] — 經驗記憶：案例、策略、技能
4. [[memory-lifecycle]] — 記憶形成、演化、檢索動態
5. [[context-engineering]] — 上下文工程與 agent memory 的區別
6. [[agent-memory-survey-summary]] — AI Agent 記憶綜述論文摘要
7. [[generative-recommendation]] — 生成式推薦新範式
8. [[large-recommendation-model]] — 大型推薦模型架構
9. [[diffusion-recommendation]] — 擴散模型推薦
10. [[conversational-recommendation]] — 對話式推薦

---

## 孤兒文章警告

> 目前所有文章均有至少一個 backlink，無孤兒文章。
