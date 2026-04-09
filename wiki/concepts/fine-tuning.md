---
title: "Fine-Tuning"
tags: [fine-tuning, training, ai-engineering]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Fine-tuning 是遷移學習（transfer learning）的核心實作方式，指的是在預訓練模型的基礎上，透過進一步訓練來調整模型權重，使其適配特定下游任務。與從零訓練不同，fine-tuning 利用模型已學到的通用知識，僅需相對少量的領域資料即可讓模型在目標任務上展現優異表現。

## 何時該微調

Fine-tuning 最適合用於行為層面的調整：統一輸出風格與格式、讓模型遵循特定指令模式、適配特定領域的專業用語與表達方式。當你發現模型「知道答案但表達方式不對」時，fine-tuning 通常是正確選擇。此外，當需要在延遲敏感場景中將大模型能力遷移至小模型時，fine-tuning 也是關鍵手段。

## 何時不該微調

如果問題出在模型缺乏特定事實或最新資訊，[[rag]] 或其他檢索增強方案通常是更佳選擇。RAG 解決的是「模型不知道什麼」的問題，fine-tuning 解決的是「模型不知道怎麼說」的問題。實務上建議先嘗試 prompt engineering，確認無法滿足需求後再考慮 fine-tuning，因為微調的成本（資料準備、訓練、評估）遠高於調整 prompt。

## 全量微調 vs. PEFT

全量微調（full fine-tuning）更新模型所有參數，能達到最佳效能但記憶體需求極高。以一個 70 億參數模型為例，僅模型權重就需約 14GB（FP16），加上梯度與最佳化器狀態，總記憶體需求可達數倍。[[peft]] 方法（如 [[lora]]、[[qlora]]）透過僅更新極小比例的參數大幅降低記憶體需求，同時維持接近全量微調的效能。

## 記憶體瓶頸分析

訓練時的記憶體佔用主要來自四個部分：模型參數、梯度、最佳化器狀態（如 Adam 需額外儲存兩組動量）、以及中間激活值。可訓練參數量乘以數值精度決定了基本記憶體需求，而 [[quantization]] 技術可以有效壓縮這些佔用。

## SFT vs. Preference Fine-Tuning

Supervised fine-tuning（SFT）使用標準的輸入-輸出配對進行訓練，而 preference fine-tuning（如 RLHF、DPO）則使用人類偏好排序來對齊模型行為。兩者通常結合使用：先做 SFT 建立基本能力，再做 preference tuning 精煉輸出品質。

## Related

- [[lora]]
- [[qlora]]
- [[peft]]
- [[quantization]]
- [[model-merging]]
- [[model-distillation]]
