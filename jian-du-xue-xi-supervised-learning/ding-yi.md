---
description: 何謂監督式學習？
---

# 定義、應用及演算法整理

### 定義

監督式學習是機器學習的一種方法，

由訓練資料中學到或建立一個模式（函數 / learning model），並依此模式推測新的實例。訓練資料是由輸入物件（通常是向量）和預期輸出所組成。函數的輸出可以是一個連續的值（稱為迴歸分析），或是預測一個分類標籤（稱作分類）。

### 實踐步驟

1. 決定訓練資料的形態
2. 蒐集訓練資料
3. 決定輸入特徵的表示法\
   因為學習函數的準確度與訓練輸入資料的表示法好壞有很大的關聯。通常會遇到高維災難，需要採用合適的向量轉換方法，如：降維算法、嵌入層模型等
4. 決定成本函數和演算法所使用的資料結構
5. 開始訓練演算法、經由交叉驗證搜尋超參數並觀察評估指標

### 注意事項

* 沒有一種演算法在所有問題上都表現最好，目前仍時常以經驗法來比較分類器的表現及決定分類器表現的「資料特性」

### 運用情境

#### (1) 迴歸（Regression） 

{% embed url="https://zh.wikipedia.org/wiki/%E8%BF%B4%E6%AD%B8%E5%88%86%E6%9E%90" %}

{% embed url="https://towardsdatascience.com/linear-regression-explained-1b36f97b7572" %}

### 定義

* 是一種統計數據分析方法，起源於最小平方法，目的在於找出一條**最能夠代表所有觀測資料的函數曲線**，用以代表因變數和自變數之間的關係
  * 參數估計法
    * 最小平方法（Ordinary Least Square Estimation, OLSE）
    * 最大概似估計（Maximum Likelihood Estimation, MLE）
* 可了解兩個或多個變數間是否相關、其相關方向與強度
* 建立數學模型輸入特定變數以預測目標變數
* 衡量自變數變化時，應變數的變化量

#### 實作演算法

* 簡單線性迴歸 Linear Regression：以單變量預測應變量、或判斷兩變數間的相關性（方向+程度）

![](<../.gitbook/assets/image (2).png>)

* 多元迴歸 Multivariable Regression

![](<../.gitbook/assets/image (4).png>)

* 多項式迴歸 Polynomial Regression：將一次項特徵轉換成高次項特徵的線性組合多項式
  * 因為導致過擬合的可能性也更高

![](<../.gitbook/assets/image (3).png>)

* 羅吉斯迴歸 Logistic Regression：非線性的對數函數
* 逐步迴歸 ：處理高維資料集的方法之一，目的是只用最少特徵最大化預測能力
*
  * 前進法：基於模型中最重要的變數，一次一個添加剩下的變數中最重要的變數。
  * 後退法：基於模型中所有變數，一次一個刪除最不重要的變數。
* 嶺回歸 Ridge(L2) Regression：在平方誤差的基礎上對特徵權重增加一調節係數 alpha，以懲罰
  * weight decay

![迴歸分析之成本函數](<../.gitbook/assets/image (8).png>)

![加上 Ridge Regression 後的成本函數 ](<../.gitbook/assets/image (7).png>)

* Lasso(L1) Regression：將 Ridge Regression 的懲罰項從 2 次項改為 1 次絕對值
  * 解決多重共線性問題
  * 降低異常值對模型的影響，並提高整體準確度
  * 可做到特徵篩選，**保留懲罰項減少時仍不影響成本函數的特徵（用這個來下方成本函數）**

![加上 Lasso Regression 後的成本函數](<../.gitbook/assets/image (5).png>)

* Elastic Net：結合 Lasso 及 Ridge 的優點
* 自迴歸 Autoregression Model (AR)
  * 統計上一種處理時間序列的方法，假設自己和自己的過去多期的線性組合可預測未來變化
  * 如果自相關係數(R)小於0.5，則不宜採用，否則預測結果極不準確
  * 較適合受自身歷史因素影響較大的經濟現象；受社會因素影響較大的則較不適合

![](<../.gitbook/assets/image (10).png>)

* 向量自迴歸 Vector Autoregression model (VAR)：擴充 AR 成可用多個特徵進行預測，常用於多變數時間序列

![](<../.gitbook/assets/image (12).png>)

* 格蘭傑因果關係 Granger Causality：驗證 x 的過去值是否顯著影響 y 的現在值
  * 檢驗步驟
    1. 準備工作：一開始使用 y 本身的落後期（lag value）來建立模型
       * [赤池訊息量準則](https://zh.wikipedia.org/wiki/%E8%B5%A4%E6%B1%A0%E4%BF%A1%E6%81%AF%E9%87%8F%E6%BA%96%E5%89%87)（Akaike information criterion, AIC） 
       * [貝氏訊息量準則](https://zh.wikipedia.org/w/index.php?title=%E8%B2%9D%E6%B0%8F%E8%A8%8A%E6%81%AF%E9%87%8F%E6%BA%96%E5%89%87\&action=edit\&redlink=1)（Bayesian information criterion, BIC）
    2. 建立用![y](https://wikimedia.org/api/rest_v1/media/math/render/svg/b8a6208ec717213d4317e666f1ae872e00620a0d)的落後期來預測![y](https://wikimedia.org/api/rest_v1/media/math/render/svg/b8a6208ec717213d4317e666f1ae872e00620a0d)的自迴歸模型。_如果時間序列_![y](https://wikimedia.org/api/rest_v1/media/math/render/svg/b8a6208ec717213d4317e666f1ae872e00620a0d)_是_[_廣義平穩_](https://zh.wikipedia.org/wiki/%E5%B9%B3%E7%A8%B3%E8%BF%87%E7%A8%8B)_的，則可以直接使用落後期。如果不平穩，就必須對不平穩的時間序列先做（一階或更多階）_[_差分_](https://zh.wikipedia.org/wiki/%E5%B7%AE%E5%88%86)_，直到得出平穩時間數列_
    3. 如果發現![y](https://wikimedia.org/api/rest_v1/media/math/render/svg/b8a6208ec717213d4317e666f1ae872e00620a0d) 的某期落後期 
       1. 在迴歸分析中具有顯著性（**根據 **[**t檢定**](https://zh.wikipedia.org/wiki/%E5%AD%B8%E7%94%9Ft%E6%AA%A2%E9%A9%97)** 的 **[**p值**](https://zh.wikipedia.org/wiki/P%E5%80%BC)** 來判斷**），且 
       2. 此落後期加入模型後可提高迴歸模型的解釋力（根據**迴歸分析的 **[**F檢定**](https://zh.wikipedia.org/wiki/%E8%BF%B4%E6%AD%B8%E5%88%86%E6%9E%90)），此落後期便留在模型中
    4. 然後進一步加入![x](https://wikimedia.org/api/rest_v1/media/math/render/svg/87f9e315fd7e2ba406057a97300593c4802b53e4)（或Δ![x](https://wikimedia.org/api/rest_v1/media/math/render/svg/87f9e315fd7e2ba406057a97300593c4802b53e4)）的落後期來擴充迴歸模型。關於平穩時間序列的要求、某期落後期留在模型中的條件，同上述![y](https://wikimedia.org/api/rest_v1/media/math/render/svg/b8a6208ec717213d4317e666f1ae872e00620a0d)的處理
    5. If and only if 沒有任何解釋變項![x](https://wikimedia.org/api/rest_v1/media/math/render/svg/87f9e315fd7e2ba406057a97300593c4802b53e4)（或Δ![x](https://wikimedia.org/api/rest_v1/media/math/render/svg/87f9e315fd7e2ba406057a97300593c4802b53e4)）的落後期被留在模型中，便無法拒絕無格蘭傑因果關係的虛無假說（ x 對 y 沒有因果關係）

{% embed url="https://zh.wikipedia.org/wiki/%E6%A0%BC%E8%98%AD%E5%82%91%E5%9B%A0%E6%9E%9C%E9%97%9C%E4%BF%82" %}

* 其他迴歸分析方法：
  * Bayesian Linear Regression

{% embed url="https://www.finereport.com/tw/data-analysis/7-huigui-ff.html" %}

{% embed url="https://medium.com/r-%E8%AA%9E%E8%A8%80%E8%87%AA%E5%AD%B8%E7%B3%BB%E5%88%97/r%E8%AA%9E%E8%A8%80%E8%87%AA%E5%AD%B8%E6%97%A5%E8%A8%98-9-%E8%BF%B4%E6%AD%B8%E6%A8%A1%E5%9E%8B%E4%BB%8B%E7%B4%B9-a49f81d81eab" %}

{% embed url="https://pyecontech.com/2019/12/10/regression/" %}



#### 挑戰

* **當變數維度遠大於資料量時，迴歸分析的效果較差**
* **對異常值非常敏感**。它會嚴重影響回歸線，並最終影響預測值。
* 多元迴歸可能遇到 Overfitting 問題

#### 特徵篩選：處理高維易 overfit 問題

來源：

* 特徵共線性
* 高維災難

1. 子集合選擇（Subset Selection）
2. 正規化（Regularization）：以外加懲罰項方式，減少 overfit 
   * Lasso 
   * Ridge
3. 資訊準則（Information Criterion, IC）

{% embed url="https://taweihuang.hpd.io/2016/09/12/%E8%AE%80%E8%80%85%E6%8F%90%E5%95%8F%EF%BC%9A%E5%A4%9A%E5%85%83%E8%BF%B4%E6%AD%B8%E5%88%86%E6%9E%90%E7%9A%84%E8%AE%8A%E6%95%B8%E9%81%B8%E6%93%87/" %}

#### 統計檢定

* [https://www.yongxi-stat.com/multiple-regression-analysis/](https://www.yongxi-stat.com/multiple-regression-analysis/)





**(2) 分類（Classification）**

有人工神經網路、支持向量機、最近鄰居法、高斯混合模型、樸素貝葉斯方法、決策樹和徑向基函數分類。

* 問題種類：Binary-class / Multi-class / Multi-label / Multi-class multi-output

{% embed url="https://scikit-learn.org/stable/modules/multiclass.html" %}





