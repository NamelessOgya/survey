## Sampling-Bias-Corrected Neural Modeling for Large Corpus
youtubeでも使われているrecomendation

## intro部分

### そもそもtwo tower modeとは
二つのNNを使ったrecommendationシステム。
片方のタワーにはuser context(行動ログなど)を入力し、
もう片方のタワーにはアイテムを入力する。
これを最終的に同じ空間にマップし、ユーザーとアイテムのベクトル類似度を最小化する形で学習を進める。

![image](https://user-images.githubusercontent.com/54636129/192722504-4be52269-2eb3-4fd8-b4da-2a57362bd22f.png)

## 問題設定
対象データセットとして以下を考える  
$[x_{i}, y_{i}, r_{i}]^{T}$  
※$r_{i}$はそれぞれのペアがマッチした際のリワード  

これら二つの変数はidやら特徴の複合ベクトルで、多次元。  

queryが与えられたときに、それに適するitemを持ってくるというのが目的。  

recomendationの話に置き換えると、、、  
$x_{i}$がユーザーのコンテクスト情報  
$y_{i}$がアイテムのコンテクスト情報となる。  


モデルのアウトプットは二つのタワーからのアウトプットベクトルの内積
$s(x, y) = <u(x, \theta), v(y, \theta)>$
をモデルからのアウトプットとする。

この時、M個のitemからyが抽出される確率を以下のように置くことにする。  
$P(y | x; \theta) = \frac{e^{s(x, y)}}{\sum_{j\in[M]e^{s(x, y_{j})}}}$  
  
lossは以下のように表す。  
$L_{T}(\theta) = -\frac{1}{T}\sum_{i\in[T]}r_{i}log(P(y | x;\theta))$  
  
### batch softmax
実際には、NNの学習はbatchで行われるので、全データに対するsoftmaxの計算は容易ではない。
そのため、batch内のみのsoftmaxを計算することでこれを代用する。  
$P_{B}(y_{i} | x_{i}; \theta) = \frac{e^{s(x_{i}, y_{i})}}{\sum_{j\in[B]}e^{s(x_{i}, y_{i})}}$  
  
youtube視聴データに対してこれをこのまま用いた場合、人気が高いitemはたくさんサンプリングされることになり、全体でのsoftmaxの結果とin batch softmaxの間で大きな偏りが生じてしまう。  
これを是正するためにロジットを以下のように修正  
$s^{c}(x_{i}, y_{i}) = s(x_{i}, y_{i}) - log(p_{j})$  
#$p_{j}$はアイテム$y_{j}$がbatchに入る確率  
  
最終的に、Lossは以下のようになる   
$L_{B}(\theta) = -\frac{1}{B}\sum_{i\in[B]}r_{i}log(P_{B}^{C}(y_{i}| x_{i};\theta))$
  
タワーから得られたベクトルは以下のように正規化・tempratureで割って用いる。
![image](https://user-images.githubusercontent.com/54636129/192750960-f74c0000-d218-4211-b13f-0a152878972f.png)


[ストリーミングデータのの中でpを如何にして推定するか書いてあったが、興味ないのでとばし]  

## 実験設定
-  rの設定  
  rの値はクリック後の動画視聴のエンゲージメントによって決定した。
  長時間視聴するほど大きな値を与える。
  
- itemベクトルの設計  
  チャンネル名やトピックなどの変数をごちゃまぜにしてembeddingしている。

-  ユーザーベクトル  
   watch historyはBag of words  
  
[ML Ops的なところもとばし]

### 評価関数  
実際に観測されたペアを$\Omega$観測されていないものを$\Omega^{c}$として以下のように定義
![image](https://user-images.githubusercontent.com/54636129/192749200-424e4391-8f27-4fad-a0ca-ea15c4cb0438.png)　　

### 実験2(youtube)
モデル
![image](https://user-images.githubusercontent.com/54636129/192751467-1e8441ce-bd37-4ba2-a4ab-75361d9dc4eb.png)  

結果
![image](https://user-images.githubusercontent.com/54636129/192751793-85ecb56e-f154-4501-b60d-00dfd2062f24.png)


## 次読む
https://arxiv.org/pdf/2008.13535.pdf　　

https://arxiv.org/pdf/2006.11632.pdf
