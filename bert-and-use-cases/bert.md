---
description: Bidirectional Encoder Representations for Transformers
---

# BERT 初探：將 BERT 加入 tf.keras 模型

{% hint style="info" %}
翻譯自 Kaggle Notebook
{% endhint %}

{% embed url="https://www.kaggle.com/abhinand05/bert-for-humans-tutorial-baseline" %}

### 起源與影響

BERT（Bidirectional Encoder Representations for Transformers）是由 Wikipedia（25億字）, BooksCorpus（8億字） 等未經標籤的大量文本資料作為 input，訓練成的模型。由於他在廣泛的 NLP task 都有良好的表現（如 Question Answering (SQuAD v1.1), Natural Language Inference (MNLI)等）已在 ML 社群引起轟動並改變許多人建構 NLP 模型的方式，同時它也啟發了後續更多語言模型的出現（如 Google’s TransformerXL, OpenAI’s GPT-2, XLNet, ERNIE2.0, RoBERTa 等）

### 介紹

* BERT 的構造是由多個 Transformer encoders 相疊而成 (multi-layer bidirectional Transformer encoder)
  * BERT base–12 layers (transformer), 12 attention heads, & 110 million parameters
  * BERT Large–24 layers, 16 attention heads and, 340 million parameters

![](<../.gitbook/assets/image (13).png>)

* BERT 與他的前代模型（OpenAI GPT）差異最大的部分是其 Bidirectional 的概念
  * Bidirectional: BERT 的 self-attention layer 是雙向的，也就是訓練時會從 token（一個字符）的上、下文學習
  * 上下文很重要是因為，同樣的字擺在不同的上下文位置，會有不同含意（contextual）
* Pre-trained 步驟對 BERT 的優異表現來說相當關鍵
*   __:warning:_ ImageNet movement??_

    > finally the biggest advantage of BERT is it brought about the **ImageNet movement** with it and the most impressive aspect of BERT is that we can fine-tune it by adding just a couple of additional output layers to create state-of-the-art models for a variety of NLP tasks.

### 文本前處理

文本資料在進入 BERT 模型做訓練前有先經過前處理：

* 句子開頭以 \[CLS] 做標記、斷句時以 \[SEP] 做標記
* Input Embedding 是 3 個部分的加總：Token, Segment, Position
  * Token: 文字本身 WordPiece Token
    * 使用 WordPiece Tokenization：以一個字作為單位，再反覆加入頻次高的組合字到模型中（ex: 音, 樂, 符, 音樂, 音符, 樂符, ...）
  * Segment: 文字屬於起頭句還是結尾句
  * Position: 文字在句中的位置

![](<../.gitbook/assets/image (14).png>)

### Pre-training

同時訓練兩個 task：

* Masked Language Model：把句子中的其中一個字蓋住，讓模型預測（最符合上下文語意）最可能是填入什麼字
* Next Sentence Prediction：給模型前一句，模型預測出下一句接續什麼最符合語意

### 使用 BERT 並在自己的模型中微調

BERT 可以用於以下任務：

1. 單句分類 Single Sequence Classification
2. 對句分類 Sequence Pair Classification
3. 問答任務 Question-Answering
4. 單句貼標 Single Sequence Tagging：是對句中的每個文字都貼標
   * ex: NER (Named Entity Recognition)

Fine-tuning 時的注意事項：

1. 超參數調整 Hyperparameter Tuning
   * 文本資料量大於 10 萬時，超參數的影響較小
   * 經驗法則累積，表現較好的參數：
     * **Dropout** – 0.1
     * **Batch Size** – 16, 32
     * **Learning Rate (Adam)** – 5e-5, 3e-5, 2e-5
     * **Number of epochs** – 3, 4
2. 資料量充足的情況下，pre-training steps 越多，精確度越好

### 將 Bert 加入 tf.Keras Model

```python
class BertTFM:
    
    def __init__(self, bert_layer, max_seq_length=128, lr=0.0001, epochs=15, batch_size=32):
        
        # BERT and Tokenization params
        self.bert_layer = bert_layer
        
        self.max_seq_length = max_seq_length        
        vocab_file = self.bert_layer.resolved_object.vocab_file.asset_path.numpy()
        do_lower_case = self.bert_layer.resolved_object.do_lower_case.numpy()
        self.tokenizer = tokenization.FullTokenizer(vocab_file, do_lower_case)
        
        # Learning control params
        self.lr = lr
        self.epochs = epochs
        self.batch_size = batch_size
        
        self.models = []
        self.scores = {}
        
        
    def build_model(self):
        
        input_word_ids = Input(shape=(self.max_seq_length,), dtype=tf.int32, name='input_word_ids')
        input_mask = Input(shape=(self.max_seq_length,), dtype=tf.int32, name='input_mask')
        segment_ids = Input(shape=(self.max_seq_length,), dtype=tf.int32, name='segment_ids')    
        
        pooled_output, sequence_output = self.bert_layer([input_word_ids, input_mask, segment_ids])   
        clf_output = sequence_output[:, 0, :]
        out = Dense(1, activation='sigmoid')(clf_output)
        
        model = Model(inputs=[input_word_ids, input_mask, segment_ids], outputs=out)
        optimizer = SGD(learning_rate=self.lr, momentum=0.8)
        model.compile(loss='binary_crossentropy', optimizer=optimizer, metrics=['accuracy'])
        
        return model
```

接著呼叫寫好的 class：

```python
model = BertTFM(bert_layer=bert_layer, max_seq_length=128, lr=0.0005, epochs=10, batch_size=32).build_model()
print(model.summary())
```

可以看到將 Bert 加在 input layer 後的模型架構：

![](<../.gitbook/assets/image (17).png>)



參考資料：

* [BERT for Humans: Tutorial+Baseline](https://www.kaggle.com/abhinand05/bert-for-humans-tutorial-baseline/notebook)
* [NLP with Disaster Tweets — EDA, Cleaning and BERT](https://www.kaggle.com/gunesevitan/nlp-with-disaster-tweets-eda-cleaning-and-bert/notebook#7.-Model)

