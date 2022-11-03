# [2021]Multi-Scale One-Class Recurrent Neural Networks for Discrete Event Sequence Anomaly Detection  

# Intro  
## 離散配列からの異常検知の難しさ  
- data inbalance  
大半が正常データであるから、異常なシークエンスの方がレアである。  
通常の分類モデルを使うことはできない。  
- イベントの離散性  
離散的なシンボルの相互関係を洗い出すのは大変    
- 配列としての性質  
  
## Deep SVDD  
### SVDD  
Support vector data description。  
One class SVNの一つ。  
これのDeep版がDeep SVDD。  
後ほど参考文献を読む。  
  
# Material/Method  
## Embedding  
one-hot表現したものをDenseに入れる。  
  
## Global Abnormal detection  
embeddingしたベクトルをそのままGRUに入れる。  
学習の際のlossはDeep SVDDにならって以下を用いる。  
![image](https://user-images.githubusercontent.com/54636129/199652553-f091101e-d391-4b35-9be1-5b6d4c589f8f.png)  
- 第一項は隠れベクトルが中心cから離れることに対して罰則を与える。 
- 第二項に関しては記載なし。Deep SVDDに書いてある？  
  
## Local Abnormal detection  
### なぜ必要か  
通常、異常がある配列は長い配列のごく一部分である。  
長い配列学習を行うとき、こういった局所的な異常性は、GRUの伝搬に伴い薄められてしまうかもしれないから。  
  
### 構造  
シークエンスを長さMのwindowで切って、それぞれに対してGRUを当てることにより、隠れベクトルを得る。  
それぞれの隠れベクトルのLossの総和が全体のLossになる。    
![image](https://user-images.githubusercontent.com/54636129/199653362-c39550ac-e6e2-4a09-ab20-2023d0518eb3.png)  
![image](https://user-images.githubusercontent.com/54636129/199653559-54e8a54b-54eb-44e1-b7b9-d479573aff3b.png)  

## Objective / Optimization  
### Loss  
![image](https://user-images.githubusercontent.com/54636129/199653709-947de5c1-d5a2-4f00-9214-2fd86d5e9d17.png)
  
## optimization  
普通にAdamでできるらしい。  
中心ベクトルcは0にするのではなくて、未トレーニング状態のモデルに全データを流して  
得られたEmbeddingの平均値を使うとトレーニングが早くなるらしい。  
  
# 実験  
## 利用データセット  
データセットとしては以下を用いる。  
- RUBis  
オークションサイトのログ。  
- HDFS  
Hadoopというサービスのログ  
当該サービスのオペレーションチームによって異常フラグがつけられている。
- BGL  
スパコンのログ  
  
## データ分割  
トレーニングデータにはnormalのみを用いる。 
vaid / testセットにはabnoemalなデータも含める  

## 評価  
Precision / recall / F1 scoreによって行う  
  
## 結果  
### ベンチマーク比較  
![image](https://user-images.githubusercontent.com/54636129/199655561-83c5788b-0096-4cae-8b03-8f96e36d2bf8.png)  
普通にprecisionもrecallも9割でていてすごい。 
### 各種ハイパーパラメータの影響   
![image](https://user-images.githubusercontent.com/54636129/199655726-a17b33cf-9ac0-445e-806c-b7f1c6b4e48c.png)  
- alpha
PR-AUCは10までは徐々に増加していくが、その後現象する。  
global情報の方が支配的と言える。  
- レイヤ数  
PR-AUCはレイヤ数の増加によって増えるわけではないことから    
多ければ多いほどいいわけではない。(過学習)  
validセットと相談しながら適切な値を選択する必要がある。  
- セル  
RNNは大きく劣るものの、LSTM / GRUの選択はPR-AUCに大きな影響を与えない。  



