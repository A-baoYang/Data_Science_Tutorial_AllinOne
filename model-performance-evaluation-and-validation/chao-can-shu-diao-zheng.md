# 超參數調整

## 目的：減少模型的泛化誤差

### 泛化誤差 (Generalization Error)

衡量模型在未知數據上的準確率的指標

#### 什麼會影響泛化誤差？

* **模型複雜度**：越複雜越容易 overfitting，越簡單則容易 under-fitting

### 如何平衡模型複雜度？

* 瞭解各項參數的意義及其對模型複雜度的影響大小、正負
* 透過上述正確的認識後，根據判斷和經驗縮小每次跑參數搜尋的範圍（加快調參速度）

