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
商品のEmbedding(グラフEmbedding)とユーザーのEmbeddingを類似度でマッチングする。  
ユーザーEmbeddiingの部分と類似度計算の部分は記載なし（たぶんシークエンスモデル/Two-tower?）
- グラフエンベッティングの基礎  
  - ユーザー情報をすべて用いるわけではなく、time-windowで切って処理する。(1時間) 
    - 計算コストの削減のため
    - ユーザーの興味も時間とともに移り変わるため  
  - ユーザーごとに、商品遷移順にノードを結合していくことでグラフを作成  
  - Deep walkによるシークエンス化
  - Skip-Gramによるシークエンスエンベッド  
  ![image](https://user-images.githubusercontent.com/54636129/202562785-16badfe4-162a-45f9-a420-0cce9ff1ea29.png)  

- side informationをつかったグラフエンベティング  
  - 商品のcold start/sparsity問題を解決するために、商品のサブカテゴリのembeddingを見る。  
    - 取引情報が少ない場合はカテゴリ情報を重視するような挙動に期待
  - それぞれのサブカテゴリに対して得られたembeddingベクトル（これはどう出すのか不明）を重みづけして足し合わせることで、
  商品ベクトルを得る。
  - ![image](https://user-images.githubusercontent.com/54636129/202564610-05b6367a-1422-4957-899c-46533a6793a9.png)  
  - ![image](https://user-images.githubusercontent.com/54636129/202565249-314ba924-fc19-42fb-bb87-202165b69555.png)
  - 

## 結果  
ABテストにて検証  

- BGE  
  グラフ情報のみのEmbedding  
- GES  
  重み情報なしのside infoを用いて学習
- EGES  
  重み付きのside infoも用いてEmbedding
- BASE  
  既存アルゴリズム（詳細不明）  
![image](https://user-images.githubusercontent.com/54636129/202568181-a20c923a-77e9-4902-ad5a-e9a01529a87f.png)

## おまけ  
ここすき  
![image](https://user-images.githubusercontent.com/54636129/202567994-9a707d83-0ea5-4f2a-991d-350d86fbedad.png)
