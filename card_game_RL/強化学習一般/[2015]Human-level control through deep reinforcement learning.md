# RECURRENT EXPERIENCE REPLAY IN DISTRIBUTED REINFORCEMENT LEARNING
url:https://training.incf.org/sites/default/files/2023-05/Human-level%20control%20through%20deep%20reinforcement%20learning.pdf

# summary  
## どんなもの  
- 強化学習エージェントはこれまで有用な特徴量を作り出せるドメインや、完全に観測可能な低次元の状態空間を持つものに限られてきた。
- 深層学習を用いて高次元の入力から直接ポリシーを学習できる、DQNを開発した。
- 画面のピクセル情報とゲームスコアのみを入力に用いて、49のゲームで人間のテスターと同程度のスコアを出すことができた。
## 先行技術と比べてどこがすごい。


## 技術や手法の特異点はどこか  
- 画像学習にはCNNを利用
- 深層学習を用いて最適価値関数
    - ${Q(s,a) = max_{pi}E[r_{t} + \gamma r_{t+1} + \gamma^{2} r_{t+2}|s_t = s, a_t = a, \pi]}$
    - ${\pi = P(a|s)}$
## 議論はあるか  


## 本文  
- 強化学習はニューラルネットワークを用いて学習すると不安定になったり発散になったりすることがある。
    - Qに対して小さなアップデートをすると、ポリシーが著しく変わることがあり、それがデータ分布とaction_value(Q)とtarget valuesの間の相関に大きな影響を与える。
    - 

