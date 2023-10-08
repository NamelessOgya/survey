# RECURRENT EXPERIENCE REPLAY IN DISTRIBUTED REINFORCEMENT LEARNING
url:https://cdn.aaai.org/ocs/11673/11673-51288-1-PB.pdf

# summary  
## どんなもの  
- 過去のフレームをCNNスタックしていた部分をRNNに変更することで単一のフレーム情報量のみで経過情報を伝搬することに成功した。
- RNNでDQNの性能を再現できた。
- 部分的に情報が隠蔽された環境においても。DQNと同等の性能を再現できた。

## 先行技術と比べてどこがすごい。
- 部分的に情報が隠蔽された環境においても。DQNと同等の性能を再現できた。

## 技術や手法の特異点はどこか  
- Q値推定モデルにRNNを使っている。

## 議論はあるか  
なし

#　Introduction　　
- 現行のDQNには限られた個数の過去情報しか使用できないという制限がある。  
    - Atariの例では直近4枚のstateを参照するのみ。
    - 4つ以上の入力を必要とするゲームは、現行DQNにとってPOMDPに見えている。
- RNNを使ってより多くのフレームを参照できるようにすることで、POMDPに強いモデルができるのではないかと考えた。  
# Deep Q-Learning  
- Q-learning
    - state / action別の価値を推定する。（この推定値をQ値と呼ぶ。）
    - Q値はiterativeに以下の式を使って更新する。
        - ${Q(s, a) = Q(s,a) + \alpha(r + \gamma max{a'}Q(s',a') - Q(s,a))}$
        - Atariのようにstateの数が多い環境においてはS×a通りのQ値をすべて記録しておくことはできないので、この部分をニューラルネットで代替する。  
            - Lossは以下の通り
                - ${L(s,a|\theta) = (r * \gamma max_{a}\hat{Q}(s', a|\theta) - Q(s, a|\theta))^2}$
                    - モデルを使って、現在の時刻の行動/状況予測したQ値と、次の時刻のデータをつかって予測したQ値の差を最小化するように学習する。
                    - ${\hat{Q}}$はパラメータを固定したネットワーク
    - データ生成とアップデートを同じモデルに対して行うので、アップデートがデータ生成に対して敵対的に働くこともありうる。安定した学習のために以下の工夫をする。
        - 経験データをメモリに保存しておいて、そこからランダムに取り出したデータで学習する。(経験再生)
        - モデルアップデートには一つ古いモデルで得られたパラメータアップデートを適用する。
            - 自分自身が生成した経験によってモデルが更新されないように。
        - RMSProp / ADADELTAなどのadaptive learningを使う。
# Partial Observability
- POMDPは以下の6つのtuppleからなる。
    - S(state)
    - A(action)
    - P(transitions)
    - R(rewards)
    - ${\Omega}$(observation)
    - O(観測常態分布)
        - observationはこの分布からのサンプリングと考える。
- 通常のQ-LearningではPOMDPの背景にあるメカニズムを解析する手段を持っていないため、観測が背景にあるメカニズムの影響を受ける場合にのみ有効であった。
    - 一般の場合においては${Q(o, a | \theta) \neq Q(s, a | \theta)}$であるから観測情報から推定したQ値は精度がよくなかった。
    - Qの推定にRNNを使うことで、このギャップが埋まることを期待する。

# DRQN Architecture  
- 既存の4つの画像分の情報をconvolutionしていた部分をRNNで処理し、フルコネレイヤをかませる。
# Stable Recurrent Updates  
- Bootstrapped Sequential Updates:
    - episodeを一つ選んで、それを最初から最後まで学習する。
    - RNNのstateは一貫して引き継がれる。（初期化しない。）
- Bootstrapped Random Updates:
    - episodeのなかのランダムなポジションからupdateをはじめて、決められたiterationだけupdateを行う。
    - update開始の都度、stateは初期化される。
- squential updateは長期間の依存性を学習できることが期待される一方、経験再生のヒューリスティックに反するという欠点がある。
- 実験の結果、両者は同じくらいのパフォーマンスだったので、より単純なrandom側を採用。