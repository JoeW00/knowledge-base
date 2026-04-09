---
title: "Data Augmentation"
tags: [data, training, methodology]
created: 2026-04-09
updated: 2026-04-09
---

## 定義

Data augmentation（資料擴增）是基於既有真實資料，透過各種變換手段生成新訓練樣本的技術。其核心理念是：對原始資料施加保留語義的變換，以擴大訓練集的規模與多樣性，進而提升模型的泛化能力。Data augmentation 是機器學習中歷史最悠久且應用最廣泛的資料處理技術之一。

## 文字領域的傳統方法

在自然語言處理中，常見的 augmentation 方法包括同義替換（synonym replacement）——以同義詞隨機替換句中的非停用詞；回譯（back-translation）——將文本翻譯成另一語言再翻譯回來，產生保留語義但措辭不同的新樣本；隨機插入、刪除與交換句中的詞語；以及使用上下文嵌入模型替換詞語。回譯尤其有效，因為它能自然地產生句法結構不同但語義一致的變體。

## 影像領域的傳統方法

影像領域的 augmentation 技術發展更為成熟。基礎方法包括翻轉（水平、垂直）、旋轉（任意角度）、裁切（random crop）、色彩調整（亮度、對比度、飽和度）、縮放與平移等幾何變換。進階方法如 Mixup（混合兩張影像的像素與標籤）、CutMix（用另一張影像的區塊替換原圖部分區域）、以及 AutoAugment（自動搜尋最佳 augmentation 策略）也廣泛應用於現代電腦視覺模型的訓練。

## 與 Synthetic Data 的區別

Data augmentation 與 [[synthetic-data]] 雖然都旨在擴充訓練資料，但兩者的本質不同。Augmentation 以真實樣本為基礎進行變換，每個新樣本都可追溯到某個原始資料點，因此保留了真實資料的統計特性與分佈。Synthetic data 則是從零生成全新的樣本（例如由 AI 模型直接產出），與任何特定的真實資料點無直接對應關係。這個區別使得 augmentation 通常面臨較低的品質風險，但擴充幅度也相對有限。

## 適用場景

Data augmentation 在幾個場景中特別有價值。資料不平衡問題中，可對少數類別進行過度取樣與 augmentation 來平衡類別分佈。小樣本學習（few-shot learning）中，augmentation 能從有限的標註資料中榨取更多訓練信號。正則化效果方面，augmentation 引入的隨機變化有助於防止模型過擬合，提升在未見資料上的泛化能力。

## Related

- [[synthetic-data]]
- [[data-quality]]
- [[data-curation]]
- [[model-collapse]]
