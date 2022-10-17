## [review] ニューラルマッチング  
![image](https://user-images.githubusercontent.com/54636129/196291236-9a74373a-ad23-4346-bd44-00ebcd2ebfe9.png)  

## 目的関数  
なにを「よいマッチングとするか」の部分  
おそらく下記すべてtwo tower modelsでの実装が可能  
### GRO(Ghost Recon Online)  
Player Enjoymentを最大化することを目的にする。  
各プレイヤーに対して、行動履歴や試合結果から試合後アンケートの結果(fun ⇒ 1 otherwise ⇒ 0)を予測するモデルを作成。  
対戦した両チームの予測値を最大化するようにマッチングさせる  

#### 課題点・限界  
- 論文内では実際のアンケートデータは使用なし。  
  これまでの行動履歴からアンケート結果を推測した「シミュレーション結果」を使って精度を評価している。  
- アンケートを取れる仕組みを実装できるかという部分が課題になる。  
  実際"Apex Legends"などのゲームではアンケートが実装されており、法律上無理というわけではなさそう。  
  https://apexlegends-leaksnews.com/thisankes/

### EOMM  
試合後のユーザー離脱率を最大化することを目的にしている。  
ユーザーが1週間以上ログインしなかった場合に「離脱」したとする。  
ユーザーの行動履歴、対戦結果から試合後に離脱するか否かを予測するモデルを作成し、  
予測離脱率が最小になるようにマッチングさせる。  

#### 課題点・限界  
- 離脱行動は試合結果にのみ依存しないと思われるので、精度が高い予測モデルが作れなそう。  
  論文内では解約リスクが高いユーザーでは予測を外している。（精度評価に必要な情報が公開されておらず不十分）      
  ![image](https://user-images.githubusercontent.com/54636129/196284383-d8da7395-ea1a-4f5a-aa8e-db6ca19207ff.png)  
- 直感的には、イベント有無で離脱予測結果がずれそうな気がしていて、時間的に剛健なモデルを作るのが難しそう。  
  
### Optmatch  
試合の相対点差を予測するモデルを作成(点差が開かない⇒実力拮抗したマッチング)  
予測相対点差を最小化するようにマッチングを行う    
![image](https://user-images.githubusercontent.com/54636129/196285472-566d0b2f-9c93-46ba-be3f-c602d4ca13b4.png)  

#### 課題点・限界  
- 点差が開かない⇒実力拮抗したマッチングという前提は正しいのか  
  ゲームによっては防御的な戦略をとる上級者プレイヤーがいた場合などに、実力が過小評価されそうである。  

## 結果の検証  
これを利用したマッチングにより、マッチング指標が改善したことをどうやって証明しているか  
マッチングの評価を行う際に課題となるのは成立していないマッチングのデータは存在しないことである。  
GRO / EOMMはこれをマッチングの際に利用した予測モデルの予測値を用いて埋め合わせているが、  
これでは実験の意味がない。(データ作成に用いたデータと予測に用いたデータが同じため)  
Optmathは実際にA/Bテストを行い、モデルの正当性を評価している。

### Ghost Recon Online/ EOMM  
ABテストは行わず。  
マッチングに使用したモデルを使って、全ユーザーのアンケート結果(GRO)、継続有無(EOMM)を予測し、  
それに対して提案手法・既存手法を使ったマッチングをした場合の精度(アンケート結果・継続有無)を計算して、  
精度改善を主張している。  
これはデータ欠損値の埋め合わせ生成するモデルとマッチングに使用するモデルが同じであるから、  
予測モデルが正しいことを前提とした結果となっており、予測モデルとマッチングモデル全体の評価としては不適当。  
  
### Optmatch  
A/Bテストを用いてマッチング改善を評価した。  
![image](https://user-images.githubusercontent.com/54636129/195833762-035ad643-cbb3-4364-b208-4ac859971671.png)  
下記のように既存のマッチングアルゴリズムと比べて相対得点差が下がり、実力が拮抗したマッチングとなった。  

## 参考  
[2013]Beyond Skill Rating: Advanced Matchmaking in Ghost Recon Online  
http://www.iro.umontreal.ca/~lisa/bib/pub_subject/language/pointeurs/gro-matchmaking-ieee.pdf  
  
[2017]EOMM: An Engagement Optimized Matchmaking Framework  
https://github.com/NamelessOgya/survey/blob/edit/%E3%83%AC%E3%82%B3%E3%83%A1%E3%83%B3%E3%83%89/%5B2017%5DEOMM-%20An%20Engagement%20Optimized%20Matchmaking%20Framework.md  
  
[2020]OptMatch: Optimized Matchmaking via Modeling the High-Order Interactions on the Arena  
https://github.com/NamelessOgya/survey/blob/edit/%E3%83%AC%E3%82%B3%E3%83%A1%E3%83%B3%E3%83%89/%5B2020%5DOptMatch_Optimized_Matchmaking_via_Modeling_the.md
