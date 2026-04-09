---
title: 激活函數 (Activation Function)
source: raw/articles/deep_learning_from_basic_to_implimentation/03-神經網路基礎/第17章-激活函數.md
tags: [neural-network, deep-learning, relu, sigmoid, nonlinearity]
created: 2026-04-09
updated: 2026-04-09
---

# 激活函數 (Activation Function)

激活函數是人工神經元計算的最後一步——將加權輸入的總和通過一個非線性數學函數轉換後輸出。這個看似微小的步驟對神經網路的能力至關重要：若沒有激活函數，無論堆疊多少層，整個網路在數學上都等價於單一線性變換，喪失表達複雜非線性關係的能力。

## 火車耦合的比喻

書中以貨運列車的耦合結構做類比：耦合器讓相鄰車廂之間能夠相對旋轉，使列車能沿彎道行駛。若去掉耦合器，所有車廂就合成一根剛性長棒，完全無法轉彎。激活函數就是神經網路中的耦合器——它在層與層之間引入非線性「關節」，賦予網路靈活彎曲、適應複雜資料形狀的能力。

## 常見激活函數

- **Sigmoid**：將輸出壓縮到 (0, 1) 區間，早期廣泛使用，但在深層網路中容易造成梯度消失。
- **Tanh**：輸出範圍 (-1, 1)，是 Sigmoid 的零中心版本，梯度消失問題略有緩解。
- **ReLU**（Rectified Linear Unit）：負值輸出為 0、正值保持原值。計算極簡單且有效緩解梯度消失，是當前深度學習的預設選擇。其變體包括 Leaky ReLU（負值給予小斜率）和 ELU。
- **Softmax**：多用於輸出層，將一組值轉換為加總為 1 的機率分布，適合多類別分類任務。

激活函數的選擇直接影響 [[backpropagation]] 的梯度流動，進而影響訓練速度與穩定性。ReLU 之所以成為標準，正是因為它在正值區間的梯度恆為 1，有效避免了深層網路中梯度逐層遞減的問題。

## Related

- [[backpropagation]]
- [[overfitting-underfitting]]
- [[convolutional-neural-network]]
- [[recurrent-neural-network]]
- [[deep-learning-from-basic-to-implementation-summary]]
