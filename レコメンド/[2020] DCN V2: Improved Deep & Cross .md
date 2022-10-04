## [2020] DCN V2: Improved Deep & Cross Network and Practical Lessons for Web-scale Learning to Rank Systems  
googleの検索システムで使われているレコメンド。　　
NNは交互作用を学習しにくいという背景から、交互作用を学習可能なNNアーキテクチャを作成し、  
精度を向上させた。  
レコメンド問題を行動確率の回帰として扱っているのは興味深い。

## intro  
### DCNとは  
ニューラルネットは二次の交互作用に関してはよく考慮できるものの、  
3次以上の交互作用に関してはうまく顧慮できないことが知られている。  
交互作用を考慮できるように設計したのがDCN  
  
### 本論文の貢献点  
- 表現力が高く、効率が良くシンプルなアーキテクチャをもつクロスモデル、DCN-V2を提案したこと  
- we propose to leverage low-rank techniques to approximate feature crosses in a subspace for better performance and latency trade-offs.  
- 合成データセットで実験を行い、従来のReLuを用いたニューラルネットは高次元の学習に適切ではないことを示した。
- Criteo and MovieLen-1M benchmark datasetsでsotaを達成  

## モデル  

### Cross Network  
![image](https://user-images.githubusercontent.com/54636129/193800630-31b0489a-5fb7-4709-9b1b-73511bdfa5d8.png)  
![image](https://user-images.githubusercontent.com/54636129/193800886-89d825fa-654d-4394-8f87-53425cd9f777.png)
クロスレイヤーをlこ重ねたネットワークはl + 1総文の交互作用を考慮できるらしい。  
実際にすべてのl + 1掛け合わせでできる関数を考慮できるわけではなく、いくつかは表現可能な関数系を用いた近似解になってしまう。  
  
### アンサンブルに関して  
Crossネットワーク部分とDence部分の組み合わせに関して、stack/pararelの両者を検討したが、  
どちらが有効かはdata dependentであった。  
結果的にはstack/pararel両者のアウトプットをスタックし、もう一度Dense層をかませることにより二つのアンサンブルを行った。  
  
### 計算コスト削減  
Low-rank techniquesを応用した計算量削減が行われているようだが、興味ないのでとばし。  

## 結果  
### 公開データセットにおけるパフォーマンス  
興味ないのでとばし  
  
### Google製品でのパフォーマンス  
#### 実験設計  
ユーザーデモグラを入力として、ユーザーリアクション（クリックなど）を予測する。  
![image](https://user-images.githubusercontent.com/54636129/193804471-e03f5979-c4d9-4a67-a54c-3c5e21819531.png)
