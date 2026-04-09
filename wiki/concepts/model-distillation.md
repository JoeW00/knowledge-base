---
title: "Model Distillation"
tags: [training, efficiency, deployment]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Model distillation（模型蒸餾）在 AI 工程語境下，指的是以大型模型（教師模型）的輸出作為訓練資料，來訓練較小的模型（學生模型）。這與傳統 knowledge distillation（知識蒸餾，透過特徵層面的知識轉移）有所區別——此處的蒸餾是在資料層面進行的，本質上是一種特殊形式的 [[synthetic-data]] 生成與利用。

## 教師-學生框架

Distillation 的基本流程是：選擇一個能力強大的教師模型（如 GPT-4），使用它針對目標任務生成大量高品質的輸入-輸出配對，再以這些資料對較小的學生模型進行 [[fine-tuning]]。學生模型因此能在特定任務上獲得接近教師模型的效能，同時保持遠低於教師模型的推理成本與延遲。

## 成功案例

Alpaca 是最早的成功案例之一，使用 GPT-3.5 生成的 52,000 條指令資料微調 LLaMA 7B，成本僅約 500 美元。Vicuna 則使用 ShareGPT 上分享的 ChatGPT 對話紀錄微調 LLaMA 13B，在評估中達到 ChatGPT 約 92% 的品質。這些案例展示了 distillation 在降低部署成本方面的巨大潛力，也催生了開源社群蓬勃的模型開發活動。

## 授權與法律考量

Model distillation 涉及重要的法律議題。OpenAI、Google 等公司的模型使用條款通常禁止使用其輸出來訓練競爭模型。例如 OpenAI 的 Terms of Use 明確限制了此類用途。然而，執行這些條款的實際難度很高，市場上仍有大量模型是透過 distillation 開發的。從業者需要審慎評估法律風險，特別是在商業應用場景中。

## 品質天花板問題

Distillation 的一個根本限制是學生模型的品質天花板受制於教師模型。學生模型無法超越教師模型在目標任務上的能力，且在實務中通常只能達到教師效能的一個子集。此外，學生模型往往更容易學到教師的表面風格（如回應格式、用語習慣）而非深層推理能力，這種「表層模仿」現象限制了 distillation 的效果上限。

## 與其他概念的關係

Model distillation 與 [[synthetic-data]] 密切相關——distillation 的過程本質上就是使用教師模型生成合成資料。同時，distillation 也面臨 [[model-collapse]] 的風險，特別是當多代蒸餾串聯（學生模型再作為下一代的教師）時，品質退化會加速。

## Related

- [[synthetic-data]]
- [[fine-tuning]]
- [[data-quality]]
- [[model-collapse]]
