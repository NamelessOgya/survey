## EOMM: An Engagement Optimized Matchmaking Framework  
EAが作ったマッチングシステムに関する論文  
コンテンツ実装に関しては記載がなくシミュレーションのみ。  
### intro  
マッチメイキングを最適化問題として解く。  
- プレイヤーのengagementを解約リスクで表現する。  
解約は1週間以上のゲームプレイ中断  
- マッチメイキングで待機しているユーザーを完全グラフとしてモデル化する。  
- engagementの表現はminimum weight perfect matchingを用いて解く  
  
モデルの概要
- スキルモデリング  
- 解約モデル  
- グラフマッチングモデル  
  
### Previous work  
- Bradley-Terry model  
    - 各プレイヤーに対してスキルを定義。  
    - (自分のスキル) / (自分のスキル + 相手のスキル)を勝率として定義する。  

- The Elo system  
    - $p_i$を平均$r_{i}$, 分散$\beta$、プレイヤーのパフォーマンスの確率変数  
    - rは期待される結果と実際の結果に応じて更新される。  
        - 勝てなそうな強い相手に勝つと大幅に更新される。  

- The Glicko system  
    - いわゆるグリコレーティング？  

### ENGAGEMENT OPTIMIZED MATCHMAKING  
#### 目的関数  
プレイヤー${p_{i}}$が${p_{j}}$とのマッチングのあとにやめる確率を、それぞれの状態sを用いて以下のように表す。  
${c_{i, j} = Pr(pが解約する | s_{i}, s_{j}) = c(s_{i}, s_{j})}$  
すると、プレイヤーの不満を最小化するマッチングは以下で定義される。  
![image](https://user-images.githubusercontent.com/54636129/194542905-2f387f9c-0bee-4a49-8316-0989ac1d9fe4.png)  
  
### 解約リスクの予想  
(興味ないのでてきとうに..)  
試合結果をoとするとき、以下の仮定を置く  
![image](https://user-images.githubusercontent.com/54636129/194544080-d289d268-368e-498c-a713-f1eee734df34.png)  
(つまり解約に影響するのは結果だけ。害悪傾向高いプレイヤーにまけたのかどうか、などは関係ありそうだがここでは考慮しない)  
勝敗部分は既存のレーティング手法（グリコレーティング等）をもちいて予測することにする。  
すると目的関数は以下の通り  
![image](https://user-images.githubusercontent.com/54636129/194544452-d33af1d9-76f5-4d49-a5b7-d79f494f40f6.png)  
  
### 最適ペアの発見  
興味ないので飛ばし、自分が使う場合はvertex ai matching engineで対応可能  
  
## 実験  
EAのPvPゲームで検証をおこなった。  
  
-  用いたデータ  
EAの有名なゲームのマッチングデータ、36.9 milion  
- player skill  
グリコレーティングの平均をレート、分散をRDにもつ正規分布を仮定。確率変数を$\mu$とする。  
![image](https://user-images.githubusercontent.com/54636129/194546115-ec7f9a84-8e1b-4a31-a899-fd8ea7823f16.png)  
グリコレーティングのアルゴリズムに従って勝利確率を予測。  
「引き分け」も出力できるようにモデルを微調整  


- Churn Prediction Model  
ロジスティック回帰を用いる。  
解約の予測の際ユーザー状態sとして以下を渡す。  
- それぞれのプレイヤーの直近10試合の結果  
- この後の試合結果  
- 過去8時間、1日、1週間、一ヶ月間の試合数  

### シミュレーション  
以下の手順でランダムマッチング・レートマッチングと比較  
- ユーザーをサンプルしてきてマッチングプールを作る  
- アルゴリズムでマッチング  
- 予測解約率を計算  
→解約率が少ないほどいいモデルと考える。  
  
## 感想  
制度が高い勝敗予測、離脱予測モデルがつくれるなら、この手法でシミュレーションをおこなうのも良さそうと感じた。  
ゲーム内実装できなくてもシミュレーションを行えるのは魅力的  
ただ、シミュレーション時のモデルとマッチング時のモデルに同じモデルを使っているので、一般性がないような気がする。  
(あくまで「試合結果予測・解約予測が正しいとするとこの結果」という話でしかない)



## 後で読む  
- アンケート満足度を使う例  
http://www.iro.umontreal.ca/~lisa/bib/pub_subject/language/pointeurs/gro-matchmaking-ieee.pdf  
- グリコレーティング  
http://www.glicko.net/research/glicko.pdf  
- MMOのレーティングシステム。余裕あれば  
https://arxiv.org/pdf/2208.07704.pdf  
- RTSのスキル評価モデル  
https://spronck.net/pubs/AIIDE2013Spronck.pdf  


## tomoくん向け  
http://alumni.soe.ucsc.edu/~bweber/pubs/Weber-IAAI-2011.pdf  
https://arxiv.org/pdf/1710.02264.pdf  
http://florent.garcin.ch/pubs/runge_cig14.pdf  


