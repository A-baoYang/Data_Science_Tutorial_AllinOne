---
description: Note of Hung-yi Lee DLHLP 2020 lecture
---

# 如何使用 BERT 於自然語言處理任務？

## Training Process

![](<../.gitbook/assets/image (38).png>)

## Models

* BERT
* BERT & PALs (Projected Attention Layers)
* ERNIE
* ELMo
* Grover

## What is Pre-train Model?

說到機器對語意的理解，我們希望把輸入的每個 token 都以對應的 embedding vector 表示，

但過去的 model 並不考慮上下文，如果字元相似則 embedding 很有可能相同，這樣的模型例如：Word2Vec, GloVe

> [Word2Vec v.s. BERT 差異](https://medium.com/swlh/differences-between-word2vec-and-bert-c08a3326b5d1)

不可能對所有詞彙列舉都給一個向量，因爲詞彙會不斷增長；那根據英文的字首、字根、字尾來生成 embedding？這樣的模型例如： FastText

或是用 CNN 以圖像方式處理中文字的部首組成，來轉換特徵？

以上方法，即使同個字在不同上下文、語意不同時，他們的 embedding vector 卻都一樣。

### Contextualized Word Embedding

同樣的字元 token 相同，對根據字元的上下文會輸出不同的 embedding vector。

常見使用的模型架構：

* LSTM
* Self-Attention Layers
* Tree-based models 
  * 將文法資訊輸入，不過這樣的做法因為沒有勝出太多不太popular，前兩者在訓練過程中其實也會學到文法資訊，無需事先輸入
  * 在文法結構特別明顯的任務中，成效才有明顯的比前兩種更好

### 模型輕量化

* Transformer-XL
* 減少 Self-Attention 的運算量
  * Reformer
  * Longformer
* 多種壓縮 BERT 的方法文獻及所使用的技術：[http://mitchgordon.me/machine/learning/2019/11/18/all-the-ways-to-compress-BERT.html](http://mitchgordon.me/machine/learning/2019/11/18/all-the-ways-to-compress-BERT.html)

## How to fine-tune

* Fine-tune 的好處（相比 train from scratch、隨機起始）
  * training loss 下降可以更快
  * generalized 能力更好

![進行 fine-tune 的模型，training loss 在更早的 epoch 就明顯下降，而且最終 loss 值更小](<../.gitbook/assets/image (53).png>)

![training loss 的下降坡度，越平緩則泛化能力越佳](<../.gitbook/assets/image (52).png>)

![](<../.gitbook/assets/image (39).png>)

### 根據任務分類 pre-train model 修改方向

* Input
  * one sentence
  * multiple sentence
* Output
  * one class
  * class for each token
  * copy from input
  * general sequence



#### Input - one sentence



#### Input - multiple sentence

* 在兩句中間加上分隔符號 \[SEP]

![Input - multiple sentence](<../.gitbook/assets/image (40).png>)

#### Output - one class

* 要在開頭加上 \[CLS]，告訴模型要輸出「與整個句子有關的 embedding」才能做文本分類相關任務
* 或是沒有 \[CLS] token，也能將句子中每個 token 的 embedding 都加上 task-specific model （例如：XXX classifier, regressor, etc）

![Output - one class](<../.gitbook/assets/image (41).png>)

#### Output - class for each token

* 把每個 token 輸出的 embedding 都讀進 task-specific model，對每個 embedding 都進行分類任務，預測一個 class

![Output - class for each token](<../.gitbook/assets/image (42).png>)

#### Output - copy from input

* 分別用兩個向量的訓練，預測起始位置、結束位置
* 向量和文本的 embedding 內積後，最大值的 token 位置為起始位置；預測結束位置的方法也相同
* 任務舉例：Extraction QA

![Output - copy from input](<../.gitbook/assets/image (43).png>)

#### \*Output - general sequence

* 怎麼把 BERT 用在 Seq2seq model 裡面？輸出一段文字，例如 機器翻譯、生成文本
* V1：壞處是 task-specific model 會沒有 pre-train 到
* V2：利用 GPT 的訓練概念，從分隔符號開始接上 task-specific model，輸出生成文本的第一個字元，再從第一個字元開始接上 task-specific model，重複執行動作。

![Output - general sequence (V1)](<../.gitbook/assets/image (45).png>)

![Output - general sequence (V2)](<../.gitbook/assets/image (44).png>)

### 兩種 fine-tune 方式

#### 1. 只 fine-tune task-specific model

![](<../.gitbook/assets/image (47).png>)



#### 2. BERT 和 task-specific model 兩個一起 fine-tune

根據文獻測試數據，第二種的成效會比第一種好

![](<../.gitbook/assets/image (46).png>)

採取第二種方法會遇到要儲存的 fine-tune 後的巨大模型的問題，透過**加入 Adaptor layer **來解決，只調整 pre-train model 的一部分參數。

### Adaptor

* 目的：減少所需儲存參數量
* 概念：Fine-tune 時才將 Adaptor 放在 Feed-forward layer 之後，調參時只調整 Adaptor 的參數

![](<../.gitbook/assets/image (49).png>)

* ICML 文獻試驗的效果：

![使用 Adaptor 概念做 fine-tune 能夠在使用少參數的情況下，維持成效不掉](<../.gitbook/assets/image (48).png>)

### Weighted Features

* BERT 每一層所抽取的資訊不同，把各層輸出做 weighted sum，作為 task-specific model 的輸入
* 各層的權重可當成 task-specific model 的參數，透過任務訓練學習最佳化

![](<../.gitbook/assets/image (50).png>)



## How to pre-train by yourself

* 最早這種能夠考慮上下文的文本特徵，是以非監督方式訓練出來的
  * 以翻譯任務做預訓練 Context Vector (CoVe)：Encoder (Language A) – Decoder (Language B)
  * Unsupervised Learning ->** Self-supervised Learning**：label 可由 input data 用 no-code 方式自己產生

![Concept of Self-supervised Learning](<../.gitbook/assets/image (54).png>)

### Self-supervised Learning

* Predict Next Token：用上文預測下一個字（使用 input data 本身產生 target label 即可）
  * 使用 LSTM 架構 (V) 可以循序方式讀入 input 
    * BiLSTM 雙向考慮上下文：concatenate （從句子開頭到 token）,（從句尾逆回到 token）兩個 LSTM；但缺點是兩個 LSTM encode 時沒有相互參考到，資訊是錯開的，只是各自 encode 後直接拼在一起
  * 使用 Self-Attention 架構，要設定哪些位置可以 attend 哪些不行，避免模型偷看到下一個字，失去訓練有效性
* Masked LM：克漏字任務
  * BERT：以字為單位
  * WWM：以詞為單位
  * ERNIE：以 entity / phrase 為單位
  * SpanBert：一次 mask 數個字 (1–10) 比較成效
    * SBO: Span Boundary Objective 讓要預測的被 mask 的部分，也參考其他 masked 但已經預測出的

![](<../.gitbook/assets/image (55).png>)

### XLNet

* Transformer-XL
  * 打亂 input 詞語的順序
  * 模型參照詞語中的哪些 token 是隨機決定的
  * 不輸入 mask token 只告訴要預測的字所在的位置

### LanguageModel-style v.s. BERT-style

BERT 缺乏 generation 能力，可能不太適合做 Seq2seq model 的任務

![](<../.gitbook/assets/image (58).png>)

那可以直接 pre-train 一個 Seq2seq model？

#### MASS / BART

* Encoder–Attention–Decoder 
* 對 input 做一定程度的破壞，例如：亂序、隨機 mask
  * **MASS**: Masked Sequence to Sequence pre-training
    * 將 input sequence 中的字隨機 mask 起來，模型要能夠將它還原
  * **BART**: Bidirectional and Auto-Regressive Transformers
    * Delete: 將 input sequence 中的字隨機 delete，模型要能夠將它還原
    * Permutation: 將 input sequence 亂序 -> 效果不太好
    * Rotation: 改變 input sequence 的起始字 -> 效果不太好
    * Text Infilling: 隨機加入 mask 在不該有的地方、或是故意放數量不對的 mask token -> 效果仍然較好

![](<../.gitbook/assets/image (59).png>)

#### UniLM

* 同時是 Encode/Decoder/Seq2seq model (encoder+decoder)
* 一個模型，沒有分 encoder/decoder 部分，由很多 self-attention layer 構成
* 因為是 Self-attention ，訓練時也要限制可以 attend 到哪些位置

![](<../.gitbook/assets/image (60).png>)

#### ELECTRA 

* 透過避開生成新的 token、**只做二元分類「判斷 token 是否被置換了」**，來**減少運算量**
* 利用 small BERT 產生要置換的字詞
* 數據顯示只需要 XLNet  1/4 的運算量可以做到一樣成效（但是他的問題比較單純）
  * XLNet 需要約 NTD 600 萬才能訓練完

### Sentence-level

原本是此字為單位，如果想要以句子為單位，輸出 embedding？

使用 Next Sentence Prediction 任務，有兩種訓練概念：

#### Skip thought

使用 Autoencoder 

![](<../.gitbook/assets/image (57).png>)

#### Quick thought

Skip thought 的進階版本，兩句子各自通過 encoder 得到 embedding；如果這兩句子是相鄰的，則讓他們 embedding vector 越相近，反之則越遠。

![](<../.gitbook/assets/image (56).png>)

在 RoBERTa 使用到 NSP 任務，但沒有特別有效

其他任務概念：

* SOP (Sentence order prediction)：看模型是否知道句子的順序正確/錯誤
  * 用於 ALBERT 、有幫助
  * 用於 structBERT (Alice) 

#### 其他模型

* 另一個 ERNIE: **E**nhanced Language **R**epresentatio**N** with **I**nformative **E**ntities
  * 特點是可以引入外部 knowledge 

## References

* \[DLHLP 2020] BERT and its family (Hung-yi Lee) - PartI: [https://www.youtube.com/watch?v=1\_gRK9EIQpc](https://www.youtube.com/watch?v=1\_gRK9EIQpc)
* \[DLHLP 2020] BERT and its family (Hung-yi Lee) - PartII: [https://www.youtube.com/watch?v=Bywo7m6ySlk](https://www.youtube.com/watch?v=Bywo7m6ySlk)
