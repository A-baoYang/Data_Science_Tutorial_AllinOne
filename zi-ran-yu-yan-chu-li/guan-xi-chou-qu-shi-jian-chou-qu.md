---
description: 'done: 2/ to-do: 1, 3,4,5,6,7'
---

# 知識圖譜技術鏈

![知識圖譜技術鏈（https://zhuanlan.zhihu.com/p/85556255）](<../.gitbook/assets/image (68).png>)

其中，知識獲取環節包含以下 NLP 技術：

![（https://zhuanlan.zhihu.com/p/85556255）](<../.gitbook/assets/image (69).png>)



### **環節二：知識獲取**

以下針對知識圖譜中『**知識獲取**』環節的 3 個 NLP 技術要點進行介紹

* 命名實體識別 NER
* 關係抽取 RE&#x20;
* 事件抽取 EE



### 命名實體識別 Named Entities Recognition

![](<../.gitbook/assets/image (71).png>)

* 目的
  * 識別文本中指定類別的實體，主要包括人名、 地名、 機構名、 專有名詞等
* 分類
  * 三大類
    * 實體類、時間類、數字類
  * 7小類
    * 人、地名、時間、組織、日期、貨幣、百分比
  * [IOB tagging schema (inside-outside-beginning)](https://en.wikipedia.org/wiki/Inside%E2%80%93outside%E2%80%93beginning\_\(tagging\))
    * BIOES
      * B: begin token of the chunk（實體的開頭字）
      * I: inside token of the chunk（實體中間的字們）
      * O: token belongs to no chunk（字不屬於任何實體）
      * E: end token of the chunk（實體的結束字）
      * S: chunk that has only 1 token（單詞實體）
* 分為兩部分
  * 實體邊界識別
  * 實體分類
* 方法演進（從舊到新）
  * 基於規則和辭典
    * 語言學家手工刻規則模板、選特定特徵（ex：統計信息、標點符號、指示詞、方向詞、中心詞）以模式與字符串相匹配
    * 分詞與詞性標註>>自制特徵詞典>>標註序列>>正則化匹配實體：規定正則模板（ex: XX地名 + XX特徵詞）
    * 缺點：需大量人力構建語言模型、系統周期較長、知識更新較慢、移植性較差
  * 基於統計的機器學習：將 NER、分詞問題當做**序列標註**問題（預測標籤之間具有強相互依賴關係）
    * 有向機率
      * HMM（隱碼可夫模型，Hidden Markov Model）
      * MEMM（最大熵碼可夫模型，MaximumEntropy）
      * SVM（支持向量機，Support Vector Machine）
    * 無向機率
      * CRF（條件隨機場，Conditional Random Fields）
        * 是 NER 的主流模型
        * 目標函數考量「輸入的狀態特徵函數」及「標籤轉移特徵函數」、為一動態規劃問題
  * 深度學習：
    * 優勢
      * 因為其對複雜非線性問題的擬合較佳，比起機器學習可以學習到更複雜的特徵，更好的完成特徵工程部分；且可搭建**端對端模型**，建構複雜的 NER 系統
    * 舉例
      * CNN-CRF
      * RNN-CRF
      * LSTM-CRF
      * BiLSTM-CRF
  * 近期
    * 注意力機制 Attention
    * Transfer Learning
    * 半監督學習
* 開源工具
  * Jiagu
  * Jieba
  * Stanford CoreNLP

{% hint style="info" %}
[端對端（end-to-end）模型](https://blog.csdn.net/happyhorizion/article/details/100607429)

> 從經典機器學習的「特徵工程」 –> 深度學習的「Representation Learning」，盡可能使模型從原始輸入到輸出，縮減人工預處理
>
> Representation Learning 透過 layers 參數設置擬合、產生特徵表示，減少人工客製的過程，更靈活有效率
>
> 不同場景的 end-to-end 詮釋不同
{% endhint %}

![從最初階模型 到 深度學習模型的演進，特徵的處理程度要求降低](<../.gitbook/assets/image (72).png>)

### 實體鏈接

* 目的
  * **實體提及**與**知識庫中對應實體**進行鏈接 ，主要解決**實體名的歧義性與多樣性問題**，是將文本中**實體名指向真實世界實體**的任務
* 方法概念
  * 傳統
    * 計算實體提及與知識庫中實體的相似度，並選取特定的實體提及的目標實體
  * 深度學習
    * 構建多類型多模態上下文及知識的統壹表示，並建模不同信息、不同證據之間的相互交互通過將不同類型的信息映射到相同的特征空間，並提供高效的端到端訓練算法
    * 包含 多源異構證據的向量表示學習、不同證據之間相似度的學習 等
* 開源工具
  * dexter2

![（https://zhuanlan.zhihu.com/p/85556255）](<../.gitbook/assets/image (70).png>)

### 實體關係抽取

* 定義
  * 知識圖譜構建與信息提取的關鍵環節，主要提取兩個或者多個實體之間的某種聯繫
* 格式
  * 三元組（實體1, 關係, 實體2）
* 方法演進（從舊到近）
  * 基於模板：用語言學家預先定義好的語言模板，在小範圍、特定領域表現好
    * 基於觸發詞的模板
    * 基於依存句法分析的模板
  * 基於機器學習：轉為監督式學習的分類問題，需要高品質的特徵工程，常見算法如 SVM, Naive Bayes
  * [遠程監督 (Distant Supervision)](https://blog.csdn.net/dalangzhonghangxing/article/details/80246885)
    * 定義：使用少量已經標註好的關係抽取 triplets，在未標注資料中，將包含那 2 個 entity 的句子，都標記成和那些已標註的 triplets 一樣的關係
    * 用途：主要用來為關係分類任務擴充資料集
    * 優點：能很快標籤資料，不像監督學習需要花費大量成本標註
    * 缺點：
      * 有可能未標注資料中，在同樣的 2 個 entity 間有新的關係，此時遠程監督就會產生錯誤標籤，需要用另外過濾的方法將對關係分類無用的句子排除（ex：sentence-level Attention）
      * 且採用這種方法，在訓練模型中就不會產生新的關係類別，都會是跟已標注的少數資料一樣的關係費別
  * 基於神經網絡 pipeline 模型
    * CR-CNN
    * Attention CNNs
    * Attention BiLSTM
* 分為兩種
  * 限定關係抽取
  * 開發域關係抽取

### 事件關係抽取

* 定義
  * 識別文本中關於事件的信息，並以結構化的形式呈現，
* 核心概念
  * 事件描述
  * 事件觸發詞（動詞或者名詞）
  * 事件元素（實體、時間和屬性等表達語義的細粒度單位組成）
  * 元素角色（角色在某件事情上面的語義關系）
  * 事件類型（事件元素和觸發詞決定事件的類別）





## 參考資料

* [知识图谱-实体抽取与实体链接](https://zhuanlan.zhihu.com/p/85567106)
* [关系抽取调研——工业界 | github](https://github.com/BDBC-KG-NLP/IE-Survey/blob/master/%E5%85%B3%E7%B3%BB%E6%8A%BD%E5%8F%96-%E5%B7%A5%E4%B8%9A%E7%95%8C.md)
  * 算法、論文、競賽、資料集整理
* [解釋 Event Detection | 知乎](https://zhuanlan.zhihu.com/p/37372355)



