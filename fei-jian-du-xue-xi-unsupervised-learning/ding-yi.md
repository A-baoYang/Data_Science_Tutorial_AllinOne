---
description: 何謂非監督式學習？和監督式不同處在於？
---

# 定義、應用及演算法整理

### 定義

非監督式學習是機器學習的一種方法，

沒有目標輸出，由演算法從輸入的資料中學習模式。常被用來瞭解資料特性或作為提升監督式學習效能的預處理步驟。

### 挑戰

由於沒有監督式學習算法所參照的 ground truth，不易判斷模型是否真的學到隱藏資料中的模式

### 運用情境

#### (1) 分群（Clustering）

* 也稱為**聚類分析**、**集群分析**，是統計資料分析的技術之一，將擁有各自相似屬性的樣本聚到各自一群，使其成為不同的群體。分群結果會給予每個樣本點一個標籤（label）。
* 演算法又分以下不同類別：
  * 階層式分群（hierarchical）：測量樣本點間的距離後，以樹狀資料結構進行聚類，又分為 Top-down（由上而下分裂） 及 Bottom-up（由下而上聚合） 兩種算法：
    * Top-down：將所有樣本點視為一個整體進行聚類切分
    * Bottom-up：從單個樣本點開始尋找鄰近相似樣本進行聚合
  * 分割式分群（partitioning）常見算法則有：
    * K-Means
    * K-Medoids
  * 基於密度的分群算法：
    * DBSCAN（Density-based spatial clustering，空間密度分群）
    * OPTICS（Ordering points to identify the clustering structure）
  * 基於機率分佈的分群算法：
    * 最大期望演算法（Expectation-Maximization Algorithm）：在機率模型中尋找參數的最大可能性估計（Maximum Likelihood Estimation），而這個機率模型依賴於無法直接觀測的潛在變量。（後面會再詳細解釋何謂「機率模型」、「最大可能性估計」、及「潛在變量」
    * 高斯混合模型（Gaussian Mixture Model, GMM）：使用多個高斯機率密度函數來更精確地量化特徵的分佈，也就是特徵分解為多個高斯機率密度函數的統計模型。
    * 貝式-高斯混合模型（Bayesian-Gaussian Mixture Model）

#### (2) 關聯規則（Association Rule）

* 在大量數據中發現變數間隱藏關係的方法，常見應用如 購物籃分析、入侵檢測系統等。
* 實作的演算法如：
  * Apriori 演算法
  * [FP-Growth 演算法](https://www.gushiciku.cn/pl/pAj1/zh-tw)：做的事情類似，但寫法上比 Apriori 效率更高

#### (3) 異常檢測（Anomaly Detection）

* 對於不符預期的模式、樣本或事件的辨識，常應用於交易詐欺、結構缺陷檢測、醫療問題、文字錯誤辨識、入侵檢測等。
  * LOF [https://zhuanlan.zhihu.com/p/28178476](https://zhuanlan.zhihu.com/p/28178476)

#### (4) 降維（Dimension Reduction）

> _以下提到的部分方法不是非監督式（如：Lasso, Ridge），但為了讓應用情境的算法整理更完整，這邊不論是否為非監督式都一律列舉。_

轉換原資料的表示方式，降低隨機特徵個數，找出一組「互不相關」的主要特徵。可再細分為兩大方法：

* 特徵選擇：從原有的大量冗余、雜訊的特徵中找出主要特徵，文獻所探討的大部分為高維迴歸分析方法。列舉有以下方法：
  * Lasso Regression：又稱「最小絕對值收斂算法」（Least Absolute Shrinkage and Selection Operator），目的是為增強統計模型預測準確性和可解釋性，同時進行特徵選擇和正規化的迴歸分析方法。
  * Ridge Regression：又稱「嶺迴歸」，
  * Elastic net：綜合了 Lasso 能夠做特徵選擇 及 Ridge 有效正規化的優勢
  * SCAD
  * SURE screening
  * PLUS



* 特徵提取：相當於「變量選擇」的一般化 (generalized) 方法；不同處在於，「特徵提取」認為所有特徵的線性組合中，只有少數幾個真正能影響目標值。列舉有以下方法：
  * 主成分分析（Principle Component Analysis, PCA）、奇異值分解
  * t-SNE [https://mropengate.blogspot.com/2019/06/t-sne.html](https://mropengate.blogspot.com/2019/06/t-sne.html)
  * 線性判別分析（Linear Discriminant Analysis, LDA）
  * 因子分析（Factor Analysis）
  * 核方法（Kernel Method）
  * 基於距離的算法，如：
    * 多維尺度分析（相似度結構分析）
    * 非負矩陣分解
    * 隨機投影法（[詹森-林登史特勞斯定理](https://zh.wikipedia.org/wiki/%E7%BA%A6%E7%BF%B0%E9%80%8A-%E6%9E%97%E7%99%BB%E6%96%AF%E7%89%B9%E5%8A%B3%E6%96%AF%E5%AE%9A%E7%90%86)）
  * 神經網絡方法：
    * 自編碼（AutoEncoder）：對一組數據學習出新的表示方法



(5)  結構化資料分析

* 隱含狄利克雷分布（Latent Dirichlet Allocation, LDA）
  * 事先定義好有限的**主題**，並透過觀察**文件**與朋友**用詞**來計算出主題之間的關聯，以及各個文件的主題分佈，如此一來，只要文件夠多，就可以有效的快速理解不同文件的主題分佈
    * [https://www.tidytextmining.com/topicmodeling.html](https://www.tidytextmining.com/topicmodeling.html)
    * [https://tengyuanchang.medium.com/%E7%9B%B4%E8%A7%80%E7%90%86%E8%A7%A3-lda-latent-dirichlet-allocation-%E8%88%87%E6%96%87%E4%BB%B6%E4%B8%BB%E9%A1%8C%E6%A8%A1%E5%9E%8B-ab4f26c27184](https://tengyuanchang.medium.com/%E7%9B%B4%E8%A7%80%E7%90%86%E8%A7%A3-lda-latent-dirichlet-allocation-%E8%88%87%E6%96%87%E4%BB%B6%E4%B8%BB%E9%A1%8C%E6%A8%A1%E5%9E%8B-ab4f26c27184)



{% hint style="info" %}
資料來源：

* [第十四章 非監督式學習](http://yltang.net/tutorial/dsml/14/)，《資料科學與機器學習》，作者：唐元亮
* [無監督學習](https://zh.wikipedia.org/wiki/%E7%84%A1%E7%9B%A3%E7%9D%A3%E5%AD%B8%E7%BF%92)｜維基百科
* [聚類分析](https://zh.wikipedia.org/wiki/%E8%81%9A%E7%B1%BB%E5%88%86%E6%9E%90)｜維基百科
* [關聯規則](https://zh.wikipedia.org/wiki/%E5%85%B3%E8%81%94%E8%A7%84%E5%88%99%E5%AD%A6%E4%B9%A0)｜維基百科
* [降維](https://zh.wikipedia.org/wiki/%E9%99%8D%E7%BB%B4)｜維基百科
* 最大期望演算法｜
* [高斯混合模型](https://zhuanlan.zhihu.com/p/30483076)，知乎，作者：戴文亮
* [Regularized Regression | 正規化迴歸- Ridge, Lasso, Elastic Net](https://jamleecute.web.app/regularized-regression-ridge-lasso-elastic/)，果醬珍珍 Jam Jam
* [11-3 LDA（線性識別分析）](http://mirlab.org/jang/books/dcpr/feLda.asp?title=11-3%20LDA%20\(%BDu%A9%CA%C3%D1%A7O%A4%C0%AAR\)\&language=chinese)，Data Clustering and Pattern Recognition (資料分群與樣式辨認)，作者：Roger Jang (張智星)
{% endhint %}
