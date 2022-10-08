## Beyond Skill Rating: Advanced Matchmaking in Ghost Recon Online  
楽しめたかの基準にアンケート結果を使う論文..　　
のはずだったが、実際にはアンケートデータを使っておらず、シミュレーションにとどまっている。  
## intro  
- skill matchingの限界  
  - レーティングは一次元的である。  
    FPSに必要なプランニングや戦闘力など多次元的な要素を考慮できない。
  - スキルの拮抗した者同士の対決が必ずしも面白いとは限らない。  
  
## NNモデル  
### match balanceの予測(Balance Net)  
下記のように、プレイヤーの勝率を最小化するようなニューラルネットを使う  
Embeddingに何の情報を使ったのかは不明  
![image](https://user-images.githubusercontent.com/54636129/194681512-b64b66fd-cf0b-4d62-a93d-346dbafa4eda.png)  
  
### プレイヤーのEnjoymentの予測(Fun Net)  
実際に楽しんだかとどうか、はアンケートや事前知識を用いて推測する。
![image](https://user-images.githubusercontent.com/54636129/194713764-e2bd7574-1ce2-45c2-8e66-2615d547a357.png)
  
### 結果  
balance net/ fun netの結果をTrue skillsと比較  
fun netの結果部分ではアンケートデータは使われておらず、単なるシミュレーションにとどまっていた。
  

## 後で読む  
- [2007]TrueSkillTM: A Bayesian Skill Rating System  
マイクロソフトのマッチングシステム。余裕あれば    
microsoft.com/en-us/research/wp-content/uploads/2007/01/NIPS2006_0688.pdf  
