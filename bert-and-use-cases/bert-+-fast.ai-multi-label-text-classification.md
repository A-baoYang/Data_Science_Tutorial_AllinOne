# BERT + Fast.ai - Multi-label text classification

## 本文任務

* 了解多標籤分類任務
* 認識 Fast.ai 
* 使用 Fast.ai 進行文本多標籤分類任務

## 何謂多標籤分類

和「二元分類」、「多類別分類」相比，「多標籤分類」是指每個樣本能夠被賦予多個 label，各個 label 是否標上樣本，由 sigmoid function 產生一個 0\~1之間的機率、設定門檻後決定。

## 認識 Fast.ai

Fast.ai 是基於 PyTorch 的深度學習函式庫，為深度學習應用提供統一的 API，使開發者更無痛上手、加速深度學習應用開發。透過 Fast.ai 可以使用統一的架構進行影像辨識、文本、時間序列、協同過濾等應用。

* 官方文件：[https://docs.fast.ai/](https://docs.fast.ai)
* 文件及影片教學：[https://course.fast.ai/](https://course.fast.ai)

## 使用 Fast.ai 進行文本多標籤分類任務

#### 使用資料集

* [Toxic Comment Classification Challenge | Kaggle ](https://www.kaggle.com/c/jigsaw-toxic-comment-classification-challenge)

此 Kaggle 比賽使用來自維基百科的留言資料，將留言標上 6 種代表負面情緒的標籤，分別是 toxic, severe_toxic, obscene, threat, insult, identity_hate，幫助平台判斷是否阻擋用戶的不當留言。



#### 安裝所需套件

```
!pip install pytorch-pretrained-bert
```

#### 引入所需套件

包含基本的 numpy, pandas, sklearn 的 train_test_\__split 及 pytorch, fastai套件

```python
import numpy as np
import pandas as pd

from typing import *
from sklearn.model_selection import train_test_split

import torch
import torch.optim as optim

from fastai import *
from fastai.vision import *
from fastai.text import *
from fastai.callbacks import *

```

#### 將參數配置存在 \`config\` 中

```python
class Config(dict):
    def __init__(self, **kwargs):
        super().__init__(**kwargs)
        for k, v in kwargs.items():
            setattr(self, k, v)
    
    def set(self, key, val):
        self[key] = val
        setattr(self, key, val)

config = Config(
    testing=False,
    bert_model_name='bert-base-uncased', # 可選用自己想要使用的 pretrained model
    max_lr=1e-02,
    epochs=1,
    use_fp16=False,
    bs=8,
    max_seq_len=128
)
```

#### 自 pretrained model 提取 BERT Tokenizer

```python
from pytorch_pretrained_bert import BertTokenizer
bert_tok = BertTokenizer.from_pretrained(
    config.bert_model_name,
)
```

#### 將 BertTokenizer 包裝成適用 fastai 的 class

```python
class FastAiBertTokenizer(BaseTokenizer):
    """Wrapper around a BertTokenizer to be compatible in fastai"""
    def __init__(self, tokenizer: BertTokenizer, max_seq_len: int=128, **kwargs):
        self._pretrained_tokenizer = tokenizer
        self.max_seq_len = max_seq_len

    def __call__(self, *args, **kwargs):
        return self

    def tokenizer(self, t:str) -> List[str]:
        """Limits the maximum sequence length"""
        return ["[CLS]"] + self._pretrained_tokenizer.tokenize(t)[:self.max_seq_len - 2] + ["[SEP]"]
```

```python
# 最終要使用的 tokenizer
fastai_tokenizer = Tokenizer(
    tok_func=FastAiBertTokenizer(bert_tok, max_seq_len=config.max_seq_len), 
    pre_rules=[], 
    post_rules=[]
)
# 以及 Vocab 物件
fastai_bert_vocab = Vocab(list(bert_tok.vocab.keys()))
```

#### 引入資料集

```python
label_cols = ['toxic', 'severe_toxic', 'obscene', 'threat', 'insult', 'identity_hate']
train = pd.read_csv('train.csv', error_bad_lines=False, engine='python')
train['comment_text'] = train['comment_text'].str.replace('([“”"¨«»®´·º½¾¿¡§£₤‘’])', '')
train, val = train_test_split(train)

#check their size
print(train.shape,val.shape)

```

#### 調用 fastai 的 TextClasDataBunch 方法，將資料集轉成可以丟進 fastai 模型的格式

```python
databunch = TextClasDataBunch.from_df('.', train, val, 
  tokenizer=fastai_tokenizer,
  vocab=fastai_bert_vocab,
  include_bos=False,
  include_eos=False,
  text_cols='comment_text',
  label_cols=label_cols,
  bs=config.bs,
  collate_fn=partial(pad_collate, pad_first=False, pad_idx=0),
)

# 確認轉換後的格式
databunch.show_batch()

```

### 產生多標籤模型架構

```python
def bert_class_split(self) -> List[nn.Module]:
 
  bert = model.bert
  embedder = bert.embeddings
  pooler = bert.pooler
  encoder = bert.encoder
  classifier = [model.dropout, model.classifier]
  n = len(encoder.layer)//3
  print(n)
  groups = [[embedder], list(encoder.layer[:n]), list(encoder.layer[n+1:2*n]), list(encoder.layer[(2*n)+1:]), [pooler], classifier]
  return groups

```

#### 引入 BERT classification pretrained model

```python
from pytorch_pretrained_bert.modeling import BertConfig, BertForSequenceClassification, BertForNextSentencePrediction, BertForMaskedLM

model = BertForSequenceClassification.from_pretrained('bert-base-uncased', num_labels=6)
```

#### 設定指標評估 function

```python
# Binary CrossEntropy with Logistic Loss
loss_func = nn.BCEWithLogitsLoss()

# accuracy with threshold (with threshold of 25%) to measure performance
acc_025 = partial(accuracy_thresh, thresh=0.25)

```

#### 調用 fastai 的 Learner 方法

```python
learner = Learner(databunch, model, loss_func=loss_func, model_dir='/temp/model', metrics=acc_025)
x = bert_class_split(model)
learner.split([x[0], x[1], x[2], x[3], x[4], x[5]])

```

#### 進行 learning rate 搜尋

```python
learner.lr_find()
learner.recorder.plot(skip_end=20)

```

#### 使用 loss 最低的 learning rate 進行模型再訓練

```python
learner.fit_one_cycle(config.epochs, max_lr=1e-03)
learner.save('fine_tuned_model')

```

#### 測試模型效果 

```python
text = 'you are so sweet' # change to 'you are son of bitch' and compare the result
learner.predict(text)

```

![](<../.gitbook/assets/image (19).png>)

![](<../.gitbook/assets/image (18).png>)

#### 模型成效指標

![](<../.gitbook/assets/image (22).png>)

