## OptMatch: Optimized Matchmaking via Modeling the High-Order Interactions on the Arena  
https://linxiagong.github.io/myPapers/KDD2020_OptMatch.pdf  
Neteaseのマッチングシステム  
  
### 全体の構成  
![image](https://user-images.githubusercontent.com/54636129/195452262-74176582-0afa-460c-b5b7-70a64b9a7bbc.png)  

## Problem definition  
### match making  
- Offline learning  
M個の試合結果からマッチに参加したユーザーの相互関係と  
試合パターンについて学習  
    
- Online learning  
player utility(例えば試合満足度など)を最大化するマッチングを計算する。  
![image](https://user-images.githubusercontent.com/54636129/195451922-5429bdf2-42e8-4f41-a49f-dd2cb13256ba.png)  
  
### 試合結果  
試合時の取得点数をoとして、以下のように定義  
![image](https://user-images.githubusercontent.com/54636129/195453145-209118f8-d89a-4535-8987-11296836b47d.png)  
  
### Graph Enbedding  
グラフの頂点Gと構成要素の重み${w_{i,j}}$が与えられたとき、Graph Embeddingはそれぞれの頂点をより低次元の空間に落とし込む。  
つまり、以下のような関数${f_{G}}$を学習する問題である。  
${f_{G}: V \rightarrow R^{d} \; where \; d << |V|}$  
  
このとき以下の関係が保たれるようにする。  
- First-order Proximity  
埋め込み後の近接度合いともとのグラフ${w_{i, j}}$の関係が保たれえるようにする。  
- Seconf-order Proximity  
ある頂点aに関して  
${p_{a} = (w_{a, 1}, ..., w_{a, |V|})}$とする。  
この時、(x, y)のsecond-order Proximityは  
${p_{a}, p_{b}}$の類似度合で評価される。  


