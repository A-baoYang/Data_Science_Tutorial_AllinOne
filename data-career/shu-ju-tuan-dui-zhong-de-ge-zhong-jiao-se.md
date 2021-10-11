# 數據團隊中的各種角色

### 6 Functions of a Data Team

![](<../.gitbook/assets/image (28).png>)

1. Data Engineer
   * 職責：
     * 資料可取得性、穩定性
     * 資料清洗、減少後續處理多費工的部分
2. Warehousing/Lake
   * 職責：storing business logic and preparing data for consumption
     * 根據商業邏輯，對資料進一步做 aggregation，方便其他部門取用
   * 補充：[Analytics Engineer](https://www.getdbt.com/what-is-analytics-engineering/) (ETL/ analytic code engineer-standardized)
3. DBA
   * 職責：
     * 地端資料庫規劃、維護、管理\
       （現在有些雲端服務已經可以做到相關功能）
4. Analytics Team/BI/Reporting
   * 職責：
     * 從 Data Warehouse 串接資料，製作儀表板，支援企業日常營運與決策
5. Data Science
   * 職責：
     * 資料產品開發、或加值現有產品功能
     * 技術領域包含： Search, Recommendation, prediction, 非結構化資料處理(影像特徵轉換、文字特徵轉換)
6. Growth/Ops
   * 職責：
     * 了解有哪些資料源能使用後，訂定與企業營運成長計畫相關的資料開發專案

{% embed url="https://www.sisense.com/blog/the-6-functions-of-a-data-team/" %}

