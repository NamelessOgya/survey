## [2019 ]BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding

## Intro
これまでのpre-trainingモデルは「一つの単語から次の単語を学習する」というやりかたでfine-tuningを行っていたため、情報を双方向性に利用できていなかった。
BERTでは学習タスクを工夫することにより、双方向性の学習を行えるpre-trainingモデルを目指す。

## BERT
### モデル構成
双方向性のtransformerを複数層重ねたもの

### imput/output 表現
![image](https://user-images.githubusercontent.com/54636129/172715305-98c1156f-662a-4d1b-87e8-41a949949eb5.png)
入力は二つの文をつげたものを用いる。
"Masked LM"タスクのために、このシークエンスの一部は置換されている。

outputは入力した二つの文章と、特別なトークン"C"である。

BERTは二つのタスクにより学習されている。
#### Masked LM
一定の確率で元のシークエンスのトークンを[MASK]に置換 OR 別のトークンに置換し、
置換部分を正しく予想できるかを見る。

#### Next Sentence Prediction
入力に用いる2文が連続する文であるか、そうでないかを判断する。

