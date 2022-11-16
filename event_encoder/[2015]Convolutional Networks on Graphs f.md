# [2015]Convolutional Networks on Graphs for Learning Molecular Fingerprints  
## GNNの歴史  
- 1997
    - 初のGNNが提案される。  
- 2005-2010 
    - RNNベースのGNNが流行る(RecGNN)  
- 2013
    - computer vision分野での成功もあり、 CNNの時代となる。(ConvGNN)  
        - the spectral-based approaches
        - the spatial-based approaches
- 最近(2019)  
    RecGNN, ConvGNNを礎にして新たなGNNが発達  
    - graph autoencoders
    - spatial-temporal graph neural networks 
## Categorization and Framework  
###  Taxonomy of Graph Neural Networks (GNNs)  
![image](https://user-images.githubusercontent.com/54636129/201639105-bb0fe819-be66-4088-887c-e742924e5256.png)  
- RecGNN  
ノードは隣り合うノードと絶えず情報をやり取りしていて、並行に達しているという仮定を置く。
- ConvGNN  
グリッドに対して用いられるconvolutionの演算をグラフに対して拡張。  
ノードvの特徴は、自身の特徴量vとその近傍の特徴量から計算される。  
- GAE(Graph auto encoder)  
グラフのの埋め込みと生成分布を学習する。  

###  Frameworks  
GNNのアウトプットはグラフの様々な粒度の分析に使える。  
- Node level  
ノードの回帰や分類に利用可能
- Edge-level  
エッジ分類や接続の予測に  
- Graph-level  

### Training Frameworks  
略  
  
##  RECURRENT GRAPH NEURAL NETWORKS  
各ノードの隠れ値hを以下の規則にしたがって更新する。    
![image](https://user-images.githubusercontent.com/54636129/201643718-6ea4a1aa-76fc-4306-8e45-d7a0a83fdd68.png)  
これを各hが変化しなくなるまで繰り返し、最終的な収束状態の各ノードの隠れ値hを出力に用いる。  
(f()は収縮写像であれば収束が約束される。)  
NNの出力値が次のセルに入力される形の、入力を一つしかとらないRNNである。  
(細かい手法に関する解説もあったが省略)  
  
## CONVOLUTIONAL GRAPH NEURAL NETWORKS  
- Spectral-based ConvGNNs  
数学の闇。。。
  
- Spatial-based ConvGNNs  


