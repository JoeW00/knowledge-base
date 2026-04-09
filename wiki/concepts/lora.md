---
title: "LoRA"
tags: [fine-tuning, peft, efficiency]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

LoRA（Low-Rank Adaptation）是一種參數高效微調方法，其核心思想是將微調過程中的權重更新矩陣 ΔW 分解為兩個低秩矩陣 A 與 B 的乘積（ΔW = A × B）。由於 A 和 B 的維度遠小於原始權重矩陣，可訓練參數量大幅縮減，通常僅為全量微調的 0.01% 至 1%，卻能達到相當接近的效能。

## 核心原理

LoRA 的理論基礎在於：大型語言模型的權重更新在微調過程中具有低秩特性，也就是說有效的更新資訊集中在少數幾個維度上。因此，與其更新完整的 d×d 權重矩陣，不如用一個 d×r 矩陣 A 與一個 r×d 矩陣 B 來近似更新量，其中 r（rank）遠小於 d。推理時將 ΔW 加回原始權重即可，不會增加任何推理延遲。

## Rank 參數的選擇

Rank 是 LoRA 最關鍵的超參數，通常在 4 到 64 之間選取。較低的 rank 帶來更高的壓縮率但表達能力有限，較高的 rank 則接近全量微調但節省幅度縮小。實務上，rank 8 至 16 是常見的起點，複雜任務可能需要更高的 rank。選擇時需根據任務複雜度與可用記憶體進行權衡。

## 應用層級

LoRA 通常應用於 Transformer 的 attention 層（query、key、value 投影矩陣），因為這些層對任務適配的影響最大。部分實作也會將 LoRA 擴展至 feed-forward 層以獲得更好的效果，但需要在效能提升與額外記憶體開銷之間取捨。

## 模組化部署

LoRA 的一大優勢是模組化：多個針對不同任務訓練的 LoRA adapter 可以共享同一個基礎模型，在推理時動態切換或甚至組合。這使得在生產環境中同時服務多個任務變得極為高效，只需儲存並載入體積極小的 adapter 權重即可。

## 變體與延伸

DoRA（Weight-Decomposed Low-Rank Adaptation）將權重分解為方向與大小兩個分量，分別進行調整，在部分基準測試中展現優於標準 LoRA 的表現。此外，[[qlora]] 結合了 4-bit [[quantization]] 與 LoRA，進一步壓縮記憶體需求。

## Related

- [[fine-tuning]]
- [[peft]]
- [[qlora]]
- [[quantization]]
- [[model-merging]]
