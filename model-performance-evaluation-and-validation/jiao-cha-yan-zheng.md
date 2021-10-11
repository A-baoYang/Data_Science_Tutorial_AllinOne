# 交叉驗證 Cross Validation

1.　**Resubstitution 自我一致性評估法**\
****用相同的資料進行訓練和測試，不佳\
所以這個方法通常只用在從訓練資料集找參數用，執行起來會比較快，但不太會用在驗證模型的優劣。

2.　**Holdout CV  **\
****最常見，從資料集中的**隨機**取得p%資料當作「訓練資料(Training data)」和剩下的(1-p)%當做「測試資料(Testing data)」（會考慮類別內比例）

3.　**Leave-one-out CV**\
****每次都將每一筆資料都視為「測試資料(Testing data)」，剩下的做為「訓練資料(Training data)」；重複直到每一筆都被當做「測試資料(Testing data)」為止

4.　**K-fold CV**\
將資料集依每一類比例都隨機分割成k個集合，然後將某一個集合當做「測試資料(Testing data)」，剩下的k-1個集合做為「訓練資料(Training data)」，如此重複進行直到每一個集合都被當做「測試資料(Testing data)」為止



以上都會重複執行 n 次，以這 n 次的平均值為 model performance 、這 n 次的標準差用來表示模型的穩定程度。



{% hint style="info" %}
References:\
．[交叉驗證(Cross-validation, CV)](https://chih-sheng-huang821.medium.com/%E4%BA%A4%E5%8F%89%E9%A9%97%E8%AD%89-cross-validation-cv-3b2c714b18db)\

{% endhint %}



