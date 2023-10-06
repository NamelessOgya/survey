# Mastering Strategy Card Game (Hearthstone) with Improved Techniques
url:http://proceedings.mlr.press/v119/parisotto20a/parisotto20a.pdf

# summary  
## どんなもの  
- 強化学習ドメインに適応可能なアーキテクチャであるthe Gated Transformer-XL (GTrXL)を開発した。

## 先行技術と比べてどこがすごい。
- transformer / LSTMよりも早く学習可能で、精度もよく、ハイパーパラメータやシード値に対して剛健だった。

## 技術や手法の特異点はどこか  
- residual connectionへのgame mechanizm適応
- layer norm層をresidual connectionの後に設置（Identitiy Map Recording）

## 議論はあるか  
なし

# intro  
- transformerは様々な分野で成果を上げてきたが、RLのドメインではagentに記憶を渡す手段としてLSTMが使われ続けてきた。この論文ではtransformerが強化学習に使えないかを検討。  
- the Gated Transformer-XL はtransformer/lstmよりも早く学習でき、よい成績を残した。その他のexternal memory architectureと比べてもよい成績を残した。  
- シードやハイパーパラメータに対する剛健性もLSTMに匹敵するものであった。

# transformer and variant
略

# Gated Transformer Architectures
- residual connectionへのgate mechanizm　　
    - gate functionとしては以下を検討
        - Highway
        - Sigmoid-Tanh
        - Gated-Recurrent-Unit-type gating
        - Gated Identity Initialization
- Identity Map Reordering  
    - layer normalizationをresidual connectionの後に設置する。
    - 通常のtransformerでは多くの情報がskip layerに流れてしまう。 
    - identitiy map recording自体は昔からある手法
    - 
# Experiments  
- GTrXLはmemory-based environmentにおいて、LTSMに対して勝り、reactive environmentにおいても劣らなかった。
- 

${\textbf{強化学習自体のアルゴリズムと関係なさそうなのでよむのやめ。}}$


