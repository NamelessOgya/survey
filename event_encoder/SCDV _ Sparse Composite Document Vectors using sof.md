## SCDV : Sparse Composite Document Vectors using soft clustering over　distributional representations
https://arxiv.org/pdf/1612.06778.pdf
Dheeraj Mekala* Vivek Gupta* Bhargavi Paranjape Harish Karnick
IIT Kanpur Microsoft Research Microsoft Research IIT Kanpur

word vectorを文書ベクトルに直すための手法。
これがよくつかわれるらしい。

## Sparce Composite Document Vectors
### word vector clustering
まずskip-gramを用いてword vectorを得る。
そしてこれらのword vectorを混合ガウスモデルを用いてソフトクラスタリングする。
この時のガウス分布の数kはハイパーパラメータであり、この論文の実験ではk = 60が用いられている。

### Document Topic-vector Formation
それぞれの単語$w_{i}$とクラスターKに対して、d次元の単語ベクトルを作成する。
得られた単語ベクトルに対し、単語ベクトルがクラスタkに属する確率を掛け算し以下の$wcv_{ik}$を得る。  
$wcv_{ik} = wv_{i} * P(c_{k}\ w_{i})$

その後、これらをk方向にconcatした後、$idf(w_i)$をかけることで$wtv_{i}$を得る。  
$wtv_{i} = idf(w_{i}) * concat(wcv_{ik})$

$wtv_{i}$を各単語ごとに足し合わせることにより、文書ベクトル$dv_{D}$を得る。  
$dv_{D} = \sum wtv_{i}$

### Sparce Document Vectors
$dv_{D}$のほとんどの成分は0に近い値になるため、
閾値を設定して、非常に小さい成分を0で置換する。
下流の実験では全体の4%より小さいものは0とした。

## 実験
### 既存手法との比較
テキスト分類を題材に、既存手法と比較を行った。

20NewsGroup datasetを用いて実験を行った。 
下流の分類モデルは2値分類にはロジスティック回帰、多値分類にはSVMを用いた。
多値分類タスクにおいて従来のどのモデルよりもパフォーマンスが高い。  
![image](https://user-images.githubusercontent.com/54636129/173178402-fd9d8f6a-9c13-48b4-ba19-5812a55c9592.png)  
![image](https://user-images.githubusercontent.com/54636129/173178439-bf06c46b-7d3f-4392-a420-32963b97c2ab.png)

### ハイパーパラメータの影響
クラスタ数はある程度大きければおよそ一定のパフォーマンスになりそう。
ベクトル次元も同様で、ある程度大きくすると頭打ちとなる。
（これは下流の分類モデルがロジスティック回帰だから？）  
![image](https://user-images.githubusercontent.com/54636129/173178483-0bbf0ec5-7b40-4c03-a47f-5dad486396a4.png)






