# RECURRENT EXPERIENCE REPLAY IN DISTRIBUTED REINFORCEMENT LEARNING
url:https://cdn.aaai.org/ocs/11673/11673-51288-1-PB.pdf

# summary  
## どんなもの  
- 過去のフレームをCNNスタックしていた部分をRNNに変更することで単一のフレーム情報量のみで経過情報を伝搬することに成功した。
- RNNでDQNの性能を再現できた。
- 部分的に情報が隠蔽された環境においても。DQNと同等の性能を再現できた。

## 先行技術と比べてどこがすごい。


## 技術や手法の特異点はどこか  


## 議論はあるか  
なし

#　Introduction　　
- 現行のDQNには限られた個数の過去情報しか使用できないという制限がある。  
    - Atariの例では直近4枚のstateを参照するのみ。
    - 4つ以上の入力を必要とするゲームは、現行DQNにとってPOMDPに見えている。
- RNNを使ってより多くのフレームを参照できるようにすることで、POMDPに強いモデルができるのではないかと考えた。  
# Deep Q-Learning  

