# Mastering Strategy Card Game (Hearthstone) with Improved Techniques
url:https://arxiv.org/pdf/2303.05197.pdf

# summary  
## どんなもの  
強化学習を用いてHearth stoneのドラフトルールの対戦エージェントを作成し、  
人間のトップクラスのプレイヤーに勝てるようになった。
## 先行技術と比べてどこがすごい。
- ゲーム内容がより複雑なhearth stoneにおいてプロ級の人間を破るまでになった。  
- これまでの論文は機械同士の対戦にのみ終始する一方で、本論文では人間相手の表かも行っている。

## 技術や手法の特異点はどこか  
- カードゲームのtimestepが高々100程度と有限であることに注目して、割引率1で学習を行った。  
- 多様なデッキ構築を許すために、CB段階で一定の確率でランダムな選択を行うようにした。
- 分散学習のVtrace部分に独自の工夫がある。${\textbf{論文読んだ後確認}}$  
- 学習段階では、対戦相手が最初に選んだカードn枚をobservation spaceに格納するようにして学習している。  

## 議論はあるか  
なし

## 研究用メモ  
- 環境からの入力情報
    - CB stage
        - observation: すべてのカード/ 選択されているカード/ 今選択肢にあるカードが表示される。
        - action: カード選択の多項分布
    - BT stage  
        - observation space: include all observable info 
        - action space:
            - カード一枚プレイのアクションには二つのアクションを要する。
                - type: {自分の手札のカード, 自分のボードのカード, 敵のボードのカード, 自分のヒロパ, ターンカード}
                - target: {自分ヒーロー, 敵ヒーロー, 自分ボード, 敵ボード}
                - 二つの組み合わせでカードをプレイする。  

## 次に読むもの  
- 本論文を引用しているreview  
    - [ ] https://arxiv.org/pdf/2305.11814.pdf  
- 参考元の論文。ここにいくらか工夫をする形で今回の実績が達成された。  
    - [ ] https://arxiv.org/pdf/2303.04096.pdf
- POMDPを解くためのメソッド
    - enlarge the observation space
        - [ ] https://openreview.net/pdf?id=r1lyTjAqYX
        - [ ] http://proceedings.mlr.press/v119/parisotto20a/parisotto20a.pdf
        - [ ] https://arxiv.org/pdf/1803.10760.pdf%22
    - learn a latent dynamic of the state
        - [ ] http://proceedings.mlr.press/v97/hafner19a/hafner19a.pdf
    - adopt a centralized critic
        - [ ] https://ojs.aaai.org/index.php/AAAI/article/download/11794/11653
- 2p0s関連の論文
    - Regret minimization
        - [ ] https://proceedings.neurips.cc/paper/2007/file/08d98638c6fcd194a4b1e6992063e944-Paper.pdf
        - [ ] http://proceedings.mlr.press/v97/brown19b/brown19b.pdf  
    -  Dual Averaging (DA) method  
        - [ ] https://ium.mccme.ru/postscript/s12/GS-Nesterov%20Primal-dual.pdf
        - [ ] https://arxiv.org/pdf/1608.07310.pdf  
        - DAが深層学習においては使いにくいことを言っている論文  
            - [ ] http://proceedings.mlr.press/v37/heinrich15.pdf
            - last-oterate convergenceに関する論文  
                - [ ] https://proceedings.neurips.cc/paper_files/paper/2021/file/77bb14f6132ea06dea456584b7d5581e-Paper.pdf
                - [ ] https://arxiv.org/pdf/2208.09855.pdf
                - [ ] https://arxiv.org/pdf/2206.05825.pdf
- hearth stone AIに関する論文  
    - tree search  
        - [ ] https://www.researchgate.net/profile/Andre-Santos-115/publication/317904983_Monte_Carlo_Tree_Search_experiments_in_Hearthstone/links/5977b48aa6fdcc30bdbadc2a/Monte-Carlo-Tree-Search-experiments-in-Hearthstone.pdf
        - [ ] https://www.skatgame.net/mburo/ps/cig17-hsai.pdf
        - [ ] https://2fyahootechpulse.easychair.org/publications/download/Kftf
    - evolutinal method
        - [ ] http://www.lcc.uma.es/~ccottap/papers/garcia19optimizing.pdf
    - RL  
        - [ ] https://arxiv.org/pdf/1808.04794.pdf
- policy gradientに使われる技術
    - V-trace
        - [ ] http://proceedings.mlr.press/v80/espeholt18a/espeholt18a.pdf
    - UPGO
        - [ ] https://www.nature.com/articles/s41586-019-1724
        (starcraft論文)
- そのほか
    - モンテカルロtreeに関してまとめ
        - [ ] https://repository.essex.ac.uk/4117/1/MCTS-Survey.pdf
    - Smooth Fictitious Playに関して ⇒ sp0s環境においてナッシュ近郊を見つけるためのメソッド
        - [ ] https://arxiv.org/pdf/1603.01121
        - [ ] https://citeseerx.ist.psu.edu/document?repid=rep1&type=pdf&doi=859d47277399c6a86312ce17e58af6bc74a16ed4 
    - starcraft論文
        - [ ] https://www.nature.com/articles/s41586-019-1724
            - an auto-regressive action space decomposition が大切らしい。
    -  the ActorLearner architecture（分散処理）  
        - [ ] http://proceedings.mlr.press/v80/espeholt18a/espeholt18a.pdf (V-traceと同一)  
        - [ ] https://arxiv.org/pdf/2011.12895.pdf
    - PPO
        - https://arxiv.org/pdf/1707.06347.pdf).
- その他調べる  
    - 強化学習の分散学習(ring / queue)

# intro  
- 完全情報ゲームではないため、ナッシュ近郊をDPで見つけるようなアプローチは取れない。
- W.Xi 2023に対して新たな工夫をする形でモデル作成した。
## hearth stoneに関して
- 今回扱う問題に関して⇒ドラフトを題材にする。  
    - pick hero(PH stage)
    - draft (CB stage)
    - battle (BT stage)
- プレイ環境にはHeathbreakerを使用。
    - mage / warrior/ hunterのみ
    - ルールは2015年次店のhearth stoneと同じ
## ralated work  
- 強化学習の側面から見たカードゲーム
    - カードゲームはPartially-Observable Markov Decision Process(POMDP)のtwo-player zero-sumの性質を持つ。
        - POMDPの解法に関しては下記が有力。
            - to enlarge the observation space
            - to learn a latent dynamic of the state
            - to adopt a centralized critic
        - 2p0sゲームにおいてナッシュ均衡を見つけ出すためにメタストラテジーが必要となる。
            - Regret minimizationは不完全情報ゲームを価値中心のやり方で解く方法として広く研究されている。
            - The Smooth Fictitious Play / Dual Averaging (DA) methodも有力だが、average-iterate convergenceしか保証されておらず、NNでの学習は難しいとされる。
- LoCM(Legends of code and magic)における試み
    -  alternating training
        - CBとBTのモデルを別々に訓練する。
    - 本論文のグループがE2E policyを開発して大きな進展があった。
        - E2Eのpolicy funcとOSFPを組み合わせている(?)
        - ${\pi_{\theta} = \delta \pi{\theta_{CB}(|o)} + (1-\delta)\pi{\theta_{BT}(|o)}}$ 
            - ${\theta_{CB}, \theta_{BT}}$はCBとBTポリシーのパラメーター
            - oは観測
## APPLICATION ON HEARTHSTONE  
- PHに関して
    - モデルの中には組み込んでいない。
- カードのプレイには2つのオペレーションが必要。
    - type
        - hand card
        - board card
        - oppenent board card
        - hero power
        - turn card
            - このカードを発動したらターン終了とみなす
    - target
        - my hero
        - opp hero
        - board card
        - opponent card
    - typeの選択によってtargetの有無が変化
- action maskを採用。
    - timestepごとに可能なアクションを指定
    - When computing the
softmax policy inside the neural net, the probabilities of those
unavailable actions are zeroed using this action mask.

- policy gradient
    - V-TraceとUPGOを併用　${\textbf{論文読んだ後確認}}$
        - ${\delta{t} = r_{t} + \gamma V_{t+1}-V_{t}}$
        - ${p_{t} = \pi(a_{t}|o_{t})/\mu(a_{t}|o_{t})}$
- meta algorithm
    - OSFPを使用(LoCMのものと同様)

##  IMPROVEMENTS AND EXPERIMENTS  
- 機械同士の評価に関して  
    - 2(=先攻後攻) × 3(=N_heros) × 3(=N_heros) × 2500試合を実施して平均勝率を算出する。

- PHに関して
    - 二つの選択肢を検討した。⇒結果的にはpolicy funcに含めなかった。 
        - 一様分布などから選定する。
        - policy functionにヒーロー選択も組み込む
            - 一つのヒーローしか選択しない方向に学習が進んでしまった。 
        
- ${\gamma}$の改善  
    - LoCMの時は0.99だった。  
    - カードゲームのエピソードの長さは高々100程度であるから、割引率を大きくとっても大丈夫なのではないか？という仮説から1.0で学習を実施
        - これにより、収束までの時間は遅くなったものの、最終的に7パーセント程度の勝率向上が見られた。
- Random Initializationによるrandom CB  
    - 直感的には、多様なデッキを用いることがBTの学習には大切である。これを実現するためにCBにランダム性を導入した。
    - 試合開始の都度、0,1,2,3の中からランダムに数字が選ばれ、この数字ごとにランダムなpickを行う。
    - これを行うことでよりhuman-likeなデッキが組まれるようになる。
- 分散学習  
    - 採用したacter learner architectureではデータの加成性が起こる。
        - ring buffer  
        learnerはリングからデータランダムにサンプルしてデータを読み取る。over-productionが起こったら古い順にデータを書き換えていく。（それが学習に使われたか使われてないかに関係なく）
        - queue buffer  
- Vtraceの改善 ${\textbf{論文読んだ後確認}}$  
- modelをヒーローごとに分離
- チート学習  
    - 対戦相手の隠された情報ももとに学習すればより強いポリシーが作れるのではないか？  
        - 対戦相手がpickした最初のn枚のカードをobservation spaceに与えてみた。
            - 相手のほうが多くのカードを見ているようでは困るので、target policiyはより多くのカードを見られるように設定した。  
        
## MACHINE-VS-HUMAN EVALUATION  
略  





