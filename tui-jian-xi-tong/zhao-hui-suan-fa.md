# 召回算法

### 常用算法

* 基於用戶行為
  * ItemCF（基於物品（被用戶的操作行為）的協同過濾）
  * UserCF
  * ALS
  * 關聯規則
* 基於內容（語言模型進行特徵抽取）
  * BERT / Transformer
  * LSTM
  * Word2Vec
  * LDA（主題分群）
  * FastText
* 深度學習
  * Youtube DNN
  * DSSM
  * TDM

### 召回流程

1. 用戶訓練樣本
2. 生成 Embedding
   1. 用戶特徵 Embedding
   2. 內容特徵 Embedding
3. 輸入模型，輸出篩選後的候選集

