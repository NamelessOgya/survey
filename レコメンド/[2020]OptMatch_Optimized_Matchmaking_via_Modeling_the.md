## OptMatch: Optimized Matchmaking via Modeling the High-Order Interactions on the Arena  
https://linxiagong.github.io/myPapers/KDD2020_OptMatch.pdf  
Neteaseのマッチングシステム  
おそらくFever basketballというのはこれのこと  
https://play.google.com/store/apps/details?id=com.netease.dunkcd&hl=en_US&gl=PH
  
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
  
## マッチングシステムの内容  
### システムの概要  
- Hero2Vec  
使用するキャラクタに対するベクトル  
- Player2Vec  
プレイヤー情報を埋め込んだベクトル。  
Hero2Vecを参照している。  
- Optmatch-net  
プレイヤーの表現を受け取り、試合結果を予測する  
  
### Hero2Vec  
ヒーロー同士の関係性を以下の2つのグラフで表す。  
おそらくMOBAのようなものが想定されている。  

#### グラフの作成
- Synergy Graph  
各ヒーローの相性をグラフ化する。  
各頂点のweightは以下のように定義される。  
![image](https://user-images.githubusercontent.com/54636129/195820204-66dc80f3-49b8-4aed-b126-18047b30643d.png)  
${f^{win}_{a,b}}$はa,bが同時に現れたゲームで勝った回数であり、loseなら負けた回数。  
  
- Suppression Graph Construction  
より直接的に、あるヒーロが別のヒーロを倒せるかどうかをグラフ化する。  
![image](https://user-images.githubusercontent.com/54636129/195820850-880c34b4-f6e1-4c78-b615-bfba8c0cdbc8.png)  

#### グラフの埋め込み  
詳しくは参照先の論文を見てくれという感じだった。  
- Line: Large-scale information network embedding. In Proceedings of the 24th international conference on world wide web.  
- Junghwan Kim, Haekyu Park, Ji-Eun Lee, and U Kang. 2018. Side: Representation learning in signed directed networks. In Proceedings of the 2018 World Wide Web Conference. 509–518.  
これを使えばグラフ関係を保ったまま、ノードをベクトル空間にmapできる、という理解で進める。  
  
### player2vec  
- Player Feature Vector  
プレイヤーの性質を表すベクトルはかなりcase specificらしい。  
ここではバスケットボールゲームの例を示す。    
![image](https://user-images.githubusercontent.com/54636129/195823696-2648f547-c791-4ee6-a84f-5d8ea46f0524.png)  

- Player Representations in the Hero Spaces.   
  ヒーローを選べるフォーマットの場合、ヒーローの好みもembedする必要がある。  
  ${p^{pick}_{h}}$を当該プレイヤーのヒーローhのpick率。  
  winは勝利マッチに限定した場合のpick率として、以下のようにベクトル化する。  
  ![image](https://user-images.githubusercontent.com/54636129/195825213-d9b70f87-5b59-403c-89db-f4fd0934a146.png)  
  
### Optmatch-Net  
Fearなマッチングが最良のものであると考える  
今回は以下の1-相対得点差を最小化することを考える。  
![image](https://user-images.githubusercontent.com/54636129/195827854-fa2d1afb-686f-4346-9bb0-9919cca67497.png)  

### input  
作成したHero2vec 2種類とplayer vectorをそれぞれinputして3種類のアウトプットを得る。  
(選んだプレイヤーのベクトルが入力される？)  

以下のようなモデルでoを予測する。    
![image](https://user-images.githubusercontent.com/54636129/195828251-60bcd64b-06e0-44bf-ad61-e7c66d677a2b.png)  
#### Team2Vec  
プレイヤーベクトルにmultihead attentionをかませる。  
  
#### Team comparison layer  
クラシカルなレーティングモデルにおいては、勝率は以下のようにモデル化される。    
![image](https://user-images.githubusercontent.com/54636129/195828993-7c622dae-a7f0-4c4e-94ab-9ee0b9e9d74a.png)...1  
これは、以下のように変形可能  
![image](https://user-images.githubusercontent.com/54636129/195829265-804453ea-b7a9-4531-9563-9a047f0cdb46.png)  
シグモイド内が多次元となる場合は以下のように重み付き和を用いることで計算する。    
![image](https://user-images.githubusercontent.com/54636129/195829495-4391faf1-a2ca-46ea-9b58-a7f7e8b46282.png)  
  
今回の相対得点差と似た性質をもつように、1.で得られた結果を用いて以下のようにoを計算する。    
![image](https://user-images.githubusercontent.com/54636129/195829808-a45c7a86-bca8-4899-afc5-f0bfe96866fa.png)  

### output layer  
最終的に得られたベクトル3つをinputにしたDenseレイヤからoがアウトプットされる。  
  
### 実験  
### 予測モデルとしてみたときの精度  
NBA/Dota/LOLに関しては勝敗のみ。  
精度は6割くらいしかない。  
自社のタイトルでも6割くらい。。。それでええんか。  
![image](https://user-images.githubusercontent.com/54636129/195832791-b5db37a4-36a2-45e7-9e89-3a062671db34.png)  
### マッチクオリティの改善  
A/Bテストを用いてマッチング改善を評価した。  
![image](https://user-images.githubusercontent.com/54636129/195833762-035ad643-cbb3-4364-b208-4ac859971671.png)  
下記のように既存のマッチングアルゴリズムと比べて相対得点差が下がり、実力が拮抗したマッチングとなった。  

## 感想  
最も詳細に記載された論文だった。  
予測精度が6割くらいであっても、マッチング改善に寄与するのは意外だった。  
やはりうまくいったかを評価するにはABテストの導入が必須だとわかった。






