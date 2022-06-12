## LogBERT: Log Anomaly Detection via BERT

## introduction / Related work
* PCA やSVMなどいった手法では、離散的ログの時間的情報をとらえるのが難しい。
* ニューラル手法ではRNNやLSTMが検討されてきたが、
* * こういった手法では右から左への情報しか獲得できず（双方向性の学習はできず）
* * これまでのメッセージから次のメッセージを予測する都合上、正しいシークエンスが突然異常シークエンスに切り替わった場合に対応できない。

これらの問題を克服するために、BERTを用いて学習を行う。

## LogBERT
ログの中からキーとなる部分を抽出する。  
![image](https://user-images.githubusercontent.com/54636129/173215029-f77c5481-99fe-4d3a-9e0b-f0d984dacc00.png)
t番目のlogのセットを$k_{t} \in K$とし、  
kのシークエンスを$S = [k_{1}, ...,k_{T} ]$  
normal log sequenceから構成されるデータセット$D = [S^{j}]^{N}_{j = 1}$とする。  

### モデルの構成
#### 入力
![image](https://user-images.githubusercontent.com/54636129/173215264-7f88e6e5-d7d9-419c-80b3-df11225fb418.png)
入力にはシークエンス$k_{t}$のembeddingをすべて足し合わせたものにposition embeddingを足したものを$x_{t}$として入力する。  
また、すべてのログの先頭には$x{DIST}$を加える。

#### Transformer Encoder
普通のやつ

#### object function
##### task 1 : Masked Log key prediction
マスクしたトークンに対して、予測値を計算する。
$\hat{y}^{j}_{[MASK]} = Softmax(W_{C}h^{j}_{[MASK]} + b_{C})$

正しいトークンとのCross entropyを計算し、その差を誤差とする。
$L_{MLKP} = -\frac{1}{N}\sum^{N}_{j = 1}\sum^{M}_{i = 1}y^{j}_{[MASK_{i}]}log\hat{y}^{j}_{[MASK_{i}]}$

##### task 2: Volume of Hypersphere Minimization
単クラス学習で用いられる、データが含まれる超球体の体積を最小化するという目的関数を今回最小化する。
シークエンスの「文」を代表する$h^{j}_{DIST}$ができるだけ小さくなるようにする。
つまり、
$c = Mean(h^{j}_{DIST})$  
$L_{VHM} = \frac{1}{N}\sum^{N}_{j=1}|| h^{j}_{DIST} - c||^{2}$

最終的にはこれら二つを考慮した、以下を誤差関数とする。
$L = L_{MLKP} +\alpha L_{VHM}$

#### BERTでどうやって異常検知を行うか
BERTは正常データのみをみてトレーニングされているので、
異常なデータを同様にマスクして予測を行った際には精度が低下すると考えられる。

正常とわかっているデータ(g個)と、異常データに対してもマスキングを行い尤度順に並べた時に
top g個に入っているデータはnormalなデータとみなす。

残りのデータに関しては尤度のworst r個を異常データとみなす。

## 結果  
これまでのどのモデルよりも精度はいいっぽい。
さすがに他のモデルたち精度悪すぎでは？
![image](https://user-images.githubusercontent.com/54636129/173223976-72ebe7ce-28dd-4393-a742-ce4265183a87.png)



## 次読むやつ
[2]Du, M., Li, F., Zheng, G., Srikumar, V.: DeepLog: Anomaly Detection and Diagnosis from System Logs through Deep Learning. In: Proceedings of the 2017 ACM SIGSAC Conference on Computer and Communications Security. (2017).
→ Deep learningをつかって異常検知  
 
 [8]Liu, F., Wen, Y., Zhang, D., Jiang, X., Xing, X., Meng, D.: Log2vec: A Heterogeneous Graph Embedding Based Approach for Detecting Cyber Threats within
Enterprise. In: Proceedings of the 2019 ACM SIGSAC Conference on Computer
and Communications Security. (2019).
 
 [17]Wang, Z., Chen, Z., Ni, J., Liu, H., Chen, H., Tang, J.: Multi-Scale One-Class
Recurrent Neural Networks for Discrete Event Sequence Anomaly Detection. In:
WSDM (2021)
→ Deep learningをつかって異常検知  
 
 [22]Zhang, K., Xu, J., Min, M.R., Jiang, G., Pelechrinis, K., Zhang, H.: Automated IT
system failure prediction: A deep learning approach. In: 2016 IEEE International
Conference on Big Data (Big Data). pp. 1291–1300 (2016).
→ Deep learningをつかって異常検知 
 
 [23]Zhou, R., Sun, P., Tao, S., Zhang, R., Meng, W., Liu, Y., Zhu, Y., Liu, Y., Pei,
D., Zhang, S., Chen, Y.: LogAnomaly: Unsupervised Detection of Sequential and
Quantitative Anomalies in Unstructured Logs. In: IJCAI. pp. 4739–4745 (2019)  
→ Deep learningをつかって異常検知  

[24]He, S., Zhu, J., He, P., Lyu, M.R.: Experience Report: System Log Analysis for
Anomaly Detection. In: 2016 IEEE 27th International Symposium on Software
Reliability Engineering (ISSRE). (2016).
→ System log anomalityに関するフレームワーク