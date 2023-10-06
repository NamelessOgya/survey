# Basic of POMDPs

# POMDPsとは？  
- Partially observable markov decision process  
- すべての状態を直接観測できない中での意思決定を行う。
- 観測できない部分は主観確率として記録しておき、状況観測の都度信念を更新する。
- POMDPsは(SAOTRO)の変数からなる。  
    - S
        - state space
    - A
        - action space
    - O
        - observation space
        - 観測空間
    - T(s'|s, a)
        - transration function
    - R(s, a)
        - Reward function
    - O(o | s')
        - observation function  
        - 観測結果から隠れ状態を予測する。
    - ${\gamma}$
        - 割引率