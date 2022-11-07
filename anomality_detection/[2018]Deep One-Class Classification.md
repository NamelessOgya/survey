# [2018]Deep One-Class Classification  
One class SVDDをDeep learningで行うというもの。

## one class学習の問題設定  
2種類の分類タスクにおいて、片方のクラスのデータしか得られない場合、もしくは片方のデータが極端に少ない場合に用いる学習方法  
異常検知などで用いられる。  

## 基本アイディア  
![image](https://user-images.githubusercontent.com/54636129/200241655-a61494d5-3cda-427b-a97d-4ca26aea5bc8.png)  
データに対してニューラルネットワークを作用させることによって、データを別空間にmapする。  
その際に、データができるだけ円中心cに集まるように学習する。  
このようにすると学習に用いていないabnormalなデータは中心cから離れた位置にmapされるので、cからの距離を測ることで正常/異常の判別が可能    
  
## 学習方法  
### 誤差関数  
- 学習データに片方のクラスしか含まれていないとき(One-class)  
  超珠の中心をcとして、以下のような誤差関数が設定される。  
  超級からの距離ができるだけ小さくなるように学習する。  
  第二項目はweight decay    
![image](https://user-images.githubusercontent.com/54636129/200242550-c5dd4919-567a-4108-bcd4-3b0cf0c15792.png)  
- 学習データにもう片方のクラスがわずかに含まれる場合(Soft-bound)  
以下のように、超珠半径Rをパラメータとして考え、これを最小化するように学習する。  
わずかに含まれる他クラスデータを許容できるようにRを超えたものには罰則を与えるような仕組みになっている。  
（こっちは興味ないのでとばし）   
![image](https://user-images.githubusercontent.com/54636129/200242389-692114cb-2758-48f0-8506-d5fbf5d33eec.png)  

### 学習の際の注意  
- 円中心に0を選ばない  
  すべての点を0にmapするようなNNが出来上がってしまうため。  
- ニューラルネットにbias項を加えない  
  同じ上。  
  bias項部分で超球中心分だけ並行移動することで↑と同じ状況が実現  
- 活性化関数にReLUを用いる  
  soft-maxなど、上下にboundaryがある関数を使うとすべての点をcにmapするNNが実現できてしまう。

## 実験・結果  
以下のデータセットに対して検証を行う  
- MINST
- CIFAR-10
- the GTSRB stop signs dataset  
  - 標識に対する敵対攻撃のデータセット  

### MINST / CIFAE-10
- CNN → Denseのアーキテクチャ  
- 10個あるクラスのうち一つをnormalとし、他のクラスの混合を異常データとする。    
![image](https://user-images.githubusercontent.com/54636129/200290393-4b270f63-280e-4abb-b725-c46f03dff2ff.png)  

- Oneclassの方がスコアがよい。（問題設定にあっているから。）


### the GTSRB stop signs dataset  
- Stop signを正常データとして、敵対攻撃データを異常データとする。  
![image](https://user-images.githubusercontent.com/54636129/200290750-030580e1-580e-427e-bd2e-26f3e7042c43.png)



