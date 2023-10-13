# RECURRENT EXPERIENCE REPLAY IN DISTRIBUTED REINFORCEMENT LEARNING
url:https://openreview.net/pdf?id=r1lyTjAqYX

# summary  
## どんなもの  
- RNNを用いた強化学習と経験再生に関して
    - 経験再生によって発生したパラメータのラグが表象ドリフト(?)や再起ステートが古くなってしまうことにつながる
    - これが分散学習環境において学習の定常性とパフォーマンスの低下につながることがわかった。
- 調査の過程で、Atariで人間を超えた最初のエージェントを開発した。

## 先行技術と比べてどこがすごい。


## 技術や手法の特異点はどこか  


## 議論はあるか  
なし

# background
## REINFORCEMENT LEARNING  
## DISTRIBUTED REINFORCEMENT LEARNING  
- 分散学習
    - 様々なactorを並列に動作させてデータを収集し、学習することに成功した。
    - Distributed replayはリプレイバッファの使用により、学習と実行を分離させることに成功した。(Ape-X agent)
    - V-traceは独立した複数のactorの情報から学習を行うアルゴリズム。データは一度しか学習に使われないため、データはlearnerのパラメータと近い値になる(?)
##  THE RECURRENT REPLAY DISTRIBUTED DQN AGENT  
- Ape-Xと同様n-step double Qlearningと複数のactorを利用した分散処理を行っている。
- (s,a,r,s')を蓄積する代わりに、(s,a,r)の固定長のsequenceを保存する。
    - 40 timestepは前のsequenceとoverrapするようにして、エピソードをまたいで記録することはしない。
    - Human-level control through deep reinforcement learningと同じように、networkをunroll
        - ↑？？？？？

${\textbf{Human-level control through deep reinforcement
learningを先に読むことに。いったん中断}}$



