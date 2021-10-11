---
description: 概述與種類
---

# 集成學習方法

## 概述

將數個弱機器學習模型通過特定策略組合後，產生一個強模型（又稱作集成評估器 ensemble estimator），比原模型表現更佳

## 算法類別

### Bagging 袋裝法

* 又稱為 Bootstrap Aggregating
* 構建多個**相互獨立**的 estimator，然後對預測做平均或多數決來決定 ensemble 結果
* 必要條件：estimators 的 accuracy 至少要 50% 以上，集成後的結果才會比單個 estimator 好
* 採取有放回的隨機抽樣
* ex：Random Forest

![圖片來源：https://medium.com/ml-research-lab/bagging-ensemble-meta-algorithm-for-reducing-variance-c98fffa5489f](<../.gitbook/assets/image (64).png>)

### Boosting 提升法

* estimator 之間按順序構建，結合弱模型的力量一次次對不同部分的樣本進行預測，逐漸構成一個強模型
* 迭代的過程，用來自適應改變訓練樣本的分布，使弱分類器聚焦到很難分類的樣本
* 給每個訓練樣本賦予一權重，在每輪訓練結束時自動調整權重
* ex：AdaBoost / GBDT

![圖片來源：https://datascience.eu/machine-learning/gradient-boosting-what-you-need-to-know/](<../.gitbook/assets/image (65).png>)

### Stacking 堆疊法

* 相對於只是將多個模型（初級學習器）的結果做平均或表決，stacking 策略是再加上一層模型（次級學習器），將弱學習器的輸出當作這最後一層模型的輸入，重新訓練這層模型，得到最終結果

![圖片來源：https://www.researchgate.net/publication/324552457\_Stacking_Ensemble_Learning_for_Short-Term_Electricity_Consumption_Forecasting/figures?lo=3](<../.gitbook/assets/image (67).png>)

##

