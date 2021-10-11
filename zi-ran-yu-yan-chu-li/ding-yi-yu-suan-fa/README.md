# 定義、應用情境及算法/套件

## 定義

自然語言處理



## 概念原理

> NLP 中的機率圖模型

{% tabs %}
{% tab title="機率論回顧" %}

{% endtab %}

{% tab title="資訊熵" %}

{% endtab %}
{% endtabs %}

* 隱馬可夫模型簡介
* 最大熵模型
* 條件隨機場模型

## 任務應用及適用算法

#### **詞典分詞**

* 切分演算法
* 字典樹
  * 停用詞過濾
  * 簡繁轉換
  * 拼音轉換
* AC 自動機演算法
* HanLP 的詞典分詞實作
* Jieba
* CKIP Tagger ([https://br19920702.medium.com/%E7%B9%81%E4%B8%AD%E9%97%9C%E9%8D%B5%E5%AD%97%E8%90%83%E5%8F%96-extract-keywords-%E9%81%8B%E7%94%A8-ckiptagger-%E8%88%87-scikit-learn-boom%E5%87%BA%E6%96%B0%E5%97%9E%E5%91%B3-3ec3e681bdec](https://br19920702.medium.com/%E7%B9%81%E4%B8%AD%E9%97%9C%E9%8D%B5%E5%AD%97%E8%90%83%E5%8F%96-extract-keywords-%E9%81%8B%E7%94%A8-ckiptagger-%E8%88%87-scikit-learn-boom%E5%87%BA%E6%96%B0%E5%97%9E%E5%91%B3-3ec3e681bdec))

#### 序列標註？

* Hidden Markov Model
* 感知機分類
* CRF 條件隨機場



#### 資訊萃取

* 新詞萃取
* 關鍵詞萃取：利用更丰富的文档外部和内部信息进行抽取會對效果更有幫助
  * 監督學習
    * SMT 機器翻譯
    * 序列标注模型（核心成分识别问题）
    * 候选词排序问题
  * 非監督學習
    * TF-IDF
      * baseline which is good enough 
    * [TextRank](https://danjtchen.medium.com/textrank-%E6%96%87%E5%AD%97%E6%8E%A2%E5%8B%98-%E6%89%BE%E5%87%BA%E9%97%9C%E9%8D%B5%E5%AD%97-%E4%BB%A5-%E5%85%AB%E5%8D%A6%E7%89%88%E6%A8%99%E9%A1%8C%E7%82%BA%E4%BE%8B-b16620370872)
      * 實際效果沒有比 TF-IDF 來得好，由于涉及网络构建和随机游走的迭代算法，效率較低
    * LDA/LSA/LSI?
    * [keyBERT](https://maartengr.github.io/KeyBERT/)
* 詞語萃取
* 關鍵句萃取



#### **命名實體識別**

* Rule based
* Machine Learning based
  * HMM
  * SVM
  * CRF
* Deep Learning based
  * Bi-LSTM-CRF
* Attention based
  * Transformer-CRF
  * BERT-BiLSTM-CRF



#### 文本摘要



#### 文本向量化

* C\&W v.s. CBOW v.s. Skip-gram v.s. unigram
* GloVe
* Word2Vec
* Doc2Vec
* Str2Vec
* BERT



#### 文本分類 / 分群



#### 情感分析

* snowNLP ([https://zhuanlan.zhihu.com/p/29747435](https://zhuanlan.zhihu.com/p/29747435))



**詞性標註**

* Hidden Markov Model
* 感知機分類
* CRF 條件隨機場



#### 依存句法分析

* PCFG?



#### 語義角色標記



#### 語料庫



#### 聊天機器人

* 問答系統
* 對話系統
* 閒聊系統

> [https://www.books.com.tw/products/0010826039?sloc=main](https://www.books.com.tw/products/0010826039?sloc=main)

#### 機器翻譯

* Seq2Seq
* Attention
* Transformer (self-attention)

#### 語言模型？



#### 神經網絡模型

* GloVe
* Word2Vec
* Doc2Vec
* LSTM
* Attention 機制
* Seq2Seq

Solr搜尋引擎?



## 套件包

* [HanLP (Python interface)](https://github.com/hankcs/pyhanlp)
* NLTK
* snowNLP
*



{% hint style="info" %}
參考資料：
{% endhint %}

* \[Book] [自然语言处理入门 Python/Java双代码实现](https://item.jd.com/12585125.html)
* \[Book] [NLP工程師養成術：自然語言處理入門](https://www.books.com.tw/products/0010862534?sloc=main)
* \[Book] [Python自然語言處理實戰：核心技術與演算法](https://www.books.com.tw/products/CN11545231?sloc=main)
* \[Book] [TensorFlow自然語言處理](https://www.books.com.tw/products/CN11664324?sloc=main)
* 推薦看精細條目： \[Book] [NLP漢語自然語言處理原理與實踐](https://www.books.com.tw/products/CN11408497?sloc=main)
* [20201009\_#1#關鍵詞提取演算法 - hackmd.io](https://hackmd.io/@NLP-Abyss/HJgLrEa8v)
* [「关键词」提取都有哪些方案？ - 知乎](https://www.zhihu.com/question/21104071/answer/24556905)
* [中文命名实体识别NER的原理、方法与工具** **- 知乎](https://zhuanlan.zhihu.com/p/156914795)
* [目前常用的自然语言处理开源项目/开发包大汇总](http://blog.itpub.net/29829936/viewspace-2221886/)
* [情感分析方法入门 - 知乎](https://zhuanlan.zhihu.com/p/300595016)

