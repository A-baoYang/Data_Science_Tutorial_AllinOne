---
description: 羅吉斯迴歸，一種常見於解決二元分類問題的迴歸方法，透過尋找最優參數來盡可能還原原始數據的分類。
---

# Logistic Regression

## 廣義線性模型

* 因線性規律的真實場景少，線性迴歸的應用有限
* 線性迴歸引入變形，通稱為「廣義線性模型」(generalized linear model)，函數稱為「聯系函數」(link function)

![](<../../.gitbook/assets/image (61).png>)

## Logistic Regression

* **是其中一種廣義線性模型**
* **g(\*) 使用對數機率函數 (Logistic function)**
* 利用線性迴歸模型的預測結果逼近正確 label 的對數機率

![](<../../.gitbook/assets/image (62).png>)

#### 優點

* 對「分類機率」建模，因此能預測 label 及其近似概率
* 繪製對數機率函數，能夠看出它屬於** Sigmoid function**
* 對數機率函數是任意階可導的凸函數，其數學性質使得很多最佳化算法都可用來求最佳解，例如：梯度下降法 (Gradient Descent)

![](<../../.gitbook/assets/image (63).png>)

## Scikit-Learn 實現與調參

```python
class sklearn.linear_model.LogisticRegression(
    penalty='l2', dual=False, tol=0.0001, C=1.0, 
    fifit_intercept=True, intercept_scaling=1, class_weight=None, 
    random_state=None, solver='warn', max_iter=100*, 
    multi_class='warn', verbose=0, warm_start=False, n_jobs=None)
```

### 正則化參數 penalty 

目的是為了解決 over-fitting，預設是 L2 正則，如果使用 L2 還是有 over-fitting，再換 L1。penalty 的選擇也會影響能夠選擇的 solver

* L1
  * 當模型的特徵非常多，希望過濾掉一些不重要的特徵、讓特徵係數（權重）歸零，使得模型係數稀疏化
  * L1 正則化的損失函數不是連續可微的，而 solver （損失函數的最佳化算法）會需要損失函數的一階或二階連續導數，因此當 penalty 設定為 L1 時，能選擇的 solver 只有 `liblinear` 
* L2

#### C 

* 搭配其他參數設置固定，透過搜尋調整

### solver

Logistic Regression 的損失函數的最佳化算法

* `liblinear`：座標軸下降法
* `Ibfgs`：擬牛頓法的一種，海森矩陣（損失函數二階導數矩陣）
* `newton-cg`：牛頓法家族的一種
* `sag`：隨機平均梯度下降法，適合樣本數據多時
  * 和普通梯度下降法的區別：每次迭代只用一部分的樣本計算梯度

### max_iter

* 找到最佳解的限制步數

### multi_class

兩者區別主要在多元分類上

* `ovr`：one-vs-rest(OvR)
  * 假設共 N 類，每次對第 K 類分類時，將 K 類以外的樣本都當成同一類，和第 K 類做二元分類；取類別對應的機率最大者為輸出類別。
  * 相對簡單、快速，但大部分情況下分類效果相對差
* `multinomial`：many-vs-many (MvM)
  * 假設共 N 類，每次在 N 類中拿兩類 N1, N2 出來，進行二元分類，共做 N(N-1)/2 次
  * MvM 分類相對精確，但速度沒有 OvR 快

