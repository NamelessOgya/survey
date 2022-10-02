## [2020]Embedding-based Retrieval in Facebook Search  
https://arxiv.org/pdf/2006.11632.pdf  
facebookで使われていたレコメンデーション  
手法の記載は抽象的だが、なにをやってうまくいかなかったか、それをどう改善したか  
が書いてあるため、実問題の試行錯誤には役立ちそうだった。

## 問題設定  
下記に示すrecallを最小化するレコメンドを良いレコメンドとみなす。  
クエリに対して得たかった結果が、モデル出力上位K個の中に何個含まれているかが評価の指標となる。  
「クエリに対して得たかった結果」はクリック数であるとか人間の評価であるとか、特定に基準によって決める  　　

![image](https://user-images.githubusercontent.com/54636129/193434923-6c0faa0f-33dd-4b00-82b9-475db498f338.png)  


## Model  
### Loss function  
![image](https://user-images.githubusercontent.com/54636129/193434690-a5fe0668-abb8-4fed-81d8-a07b64730ea7.png)  
上式のdistanceは[1 - cosine類似度]で表される。  
  
## training data mining  
  

Loss部分で定義されているnegatives（マッチングするのに好ましくないサンプル）を回収する際、  
以下の2つを検討した。  
- 1.ランダムなドキュメントをネガティブサンプルとして用いる。
- 2.従来の検索アルゴリズムで表示されたものの、クリックされなかったものをネガティブサンプルとして用いる  
  
2.方法は1.に比べて精度がよくなかった。  
また、positivesの回収の際は以下の2つを検討した。  
- 1.クリックされたものをpositiveとする。  
- 2.ユーザーに提示された(みられた)ものをpositiveとする。  
  
両者検討した結果、データ数が同じ場合制度が変わらなかったこと、  
1.のデータにみられたデータを足しても特に精度向上は見られなかったことから、  
今回のケースでは1.を採用とした。  

## Feature Enginineering  
### text features  
character n-gramを用いる。word-ngramと比べた時の利点は以下  
- embedingのlookup tableが小さくなる。  
- スペルミスなどに対してロバストになる。  
  
### location  
- query...searcher city, region, country, and language  
- document...publicly available information, such as explicit group location
tagged by the admin  
  
### social embedding  
userとentityのグラフをembeddingするモデルを別に設計し、特徴量に加えた。
  
[ベクトル探索部分は飛ばし]  
　　
### 下流の最適化
取ってきたベクトルをもとに、ランキングやフィルタリングをする工程の話。  
わからないので飛ばし  
  
## Advenced Detail  
### Hard mining  
トレーニングデータの生成の際には多様なデータを可能な限り代表するようなデータ抽出が必要である。  
classなどが定義されているcomputer visionの分野においてはHard miningの考え方があるが、  
face bookのデータにはclasラベルがないので、この考え方を応用することはできない。  

- Hard negative mining  
Negative sampleを完全にランダムに取ってくる場合、判別が簡単すぎてモデルがすべての特徴量を使ってくれず、名前だけを基準に判別してしまう現象がみられた。  
これを解決するために、Negative sampleを以下のようにサンプリングした。
  - online hard negative mining  
  各Batchの構成要素に対して、バッチないのでデータからembedding空間上でそれと一番近いデータを持ってくることでNegativeデータを作成する。  
  - Offline hard negative mining  
  全データから、政界データに最も近いデータを持ってくる方法を検討したがうまくいかず、  
  101 - 500番目に近いデータを持ってきた方がうまくいった。  
  とはいえ、自明に判断できるnegativesも必要と考え、ランダムにサンプルしたドキュメントのいくらかもnegativesに加えた。  

- アンサンブルモデルング  
それぞれことなる基準でサンプリングした学習データで学習したモデルをモデルをアンサンブルした方が精度が高くなった。  
V, Qはそれぞれクエリ、文書のembeddingとして、  
両者の類似度を以下のように定義する。  
![image](https://user-images.githubusercontent.com/54636129/193441018-c1a83425-1297-4fa7-ae74-d0ba4d01576a.png)  
![image](https://user-images.githubusercontent.com/54636129/193440899-2e27afa6-6c23-4871-9a8f-32822083b844.png)  






  


