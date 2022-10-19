## QuickSkill: Novice Skill Estimation in Online Multiplayer Games  
https://arxiv.org/pdf/2208.07704.pdf
tencentの論文。  
既存のレーティングシステムが収束するまでに18試合程度かかってしまうという問題点を解決するために、  
試合中のスナップショットデータを使って将来のMMR(レート)を予測する。  

## Quick skill
### Quick skill概要  
- Offline training  
    ある初心者ユーザーが行ったi試合に関して  
    - ユーザーの試合に於けるゲーム内状態(3分おきに取得)  
    - K試合後のMMR
    を教師データとして、MMR予測モデルを作成する。  
  
- Online Serving  
    Offline trainingで学習したモデルをMMRの代わりにつかってマッチングを行う。  

### 学習に用いる特徴量  
以下を3分おきに取得している。   
(3分おきに取るのはMOBAはゲームの進行段階によって重視される行動がことなるため)   
以下をはじめとして、snapshotごとに115の特徴を取得している。  
- 個人統計  
  - kill 
  - death
  - gold
- チームメイト/相手チームの統計  
  - 平均キル  
  - 平均death
  - 平均gold 
- その他  
  - チームの使用キャラクター情報  

### ネットワーク構造  
![image](https://user-images.githubusercontent.com/54636129/196683391-cb0ec644-4273-4361-93b8-9efc807e0e5b.png)  

#### embedding層  
- line-up embadding  
  チームがコントロールするキャラクター   
- game feature embedding  
  上に上げた統計値のembedding
- position embedding  
  positional encodingみたいなもの  
  データが何分時点のものなのかをencodingする。  
これらのembeddingデータはconcatされ、OmniNetsに送られる。  

#### OmniNets  
Transformerよりも特徴をつかみやすいネットワークらしい。  
別途勉強.  
  
#### Features Aggregation and Prediction  
transformerの出力をDenseレイヤに入れて、MMRの予測値を出力する。  
K試合後のMMRとモデルの出力した予測MMRの最小二乗誤差を最小化するように学習を行う。  
  
#### Ops  
ミリオンスケールのデータを学習するためには8つのNvidia V100で6時間かかる。(ぴぇ...)  
一度トレーニングしたモデルはデータとリフトに対応できるように、数週に一度再trainingする。  
  
これらのモデルは
- 変数の前処理のためのコンテナ10こ(4 cores / 2G memory)  
- TF-servingのために15コンテナ(4 cores / 2G memory)  
をもちいて稼働している。  

平均レスポンスは20ms程度  


## 結果  
### 経過試合数ごとの勝率差  
MMRは個人の実力指標なので、チームのMMR合計値が高い方が勝率が高くなるはず。  
⇒MMRの差が極端なマッチングほど勝率差がでているようなMMR評価が好ましい。  

各試合数時点でのTrueskill(既存手法),Quickskillで計算したMMR差ごとに、  
勝率を集計したのが下記  
![image](https://user-images.githubusercontent.com/54636129/196700196-07a16c2b-4846-4f9d-b069-e367d4fe3e07.png)  
trueskillは試合数が少ない時点では傾きがなだらか(=つまりMMRの差が大きくても勝率が大きく違わない)  
一方でQuickSkillでは試合数が少ない段階でも、傾きが急なグラフになっている。(=つまりMMRが大きく開くほど、勝率差も開く。)  





