# [2018]Billion-scale Commodity Embedding for E-commerce Recommendation in Alibaba  
中国で人気のC2CショッピングアプリTaobao(メルカリみたいなもの？)のレコメンデーション。  
https://arxiv.org/pdf/1803.02349.pdf
  
## challenge  
- 課題
  - Scalability  
    既存のアプローチは一千万単位のデータで動くが、
    億単位のデータでは動かない。
  - Sparsity  
    閲覧数が少ないユーザーや商品に関して、レコメンデーションを行うのが難しかった。  
  - Cold Start  
    取引履歴がない/ほとんどないユーザーに対して適切なレコメンドを行うのが難しかった。  
    
## Frame work  
- ユーザーグラフの構築  
  - ユーザー情報をすべて用いるわけではなく、time-windowで切って処理する。(1時間) 
    - 計算コストの削減のため
    - ユーザーの興味も時間とともに移り変わるため
  



