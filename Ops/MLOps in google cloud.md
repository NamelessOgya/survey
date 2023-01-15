# Machine Learning Operations (MLOps):Overview, Definition, and Architectureの思想をGoogleサービスで実現する。　　
- Vertex AI 機能を使って実装する場合、tooklitには何を使うことになるかを検討する。  
  - サービスの列挙を目的にする。
- 機械学習モデルをAPIとして使用する場合(matching engineなど)と分析用途での使用は分けて考える。  
- 一旦、費用は度外視する。(∵サービスを列挙する目的があるため)

# C1 CI/CD automation  
⇒ **Vertex AI Experiments**でのモデルクオリティテスト、**Vertex AI Feature Store**でのデータブレ検出、pipeline内での学習データ存在確認

論文内には以下のように記載されている。  
> The CI/CD component
ensures continuous integration, continuous delivery, and
continuous deployment. It takes care of the build, test, delivery, and
deploy steps. It providesrapid feedback to developers regarding the
success or failure of certain steps, thus increasing the overall
productivity [10,15,17,26,35,46] [α, β, γ, ε, ζ, η]. Examples are
Jenkins [17,26] and GitHub actions (η).

- ビルド、テスト、デリバリとデプロイを行い、それぞれの成否をフィードバックする。
- DevOpsの文脈ではテストは作成したコードに対して行われるが、「テスト」部分はモデルのクオリティテストや学習データのテストも含む。  
- そもそもコードに対してテストって必要？  
  - 理由1. 機械学習部分においてはエラーが出て止まってしまってもよい。  
    - 学習部分でエラーが出てしまっても、下流のデプロイ工程が実施されないようにすれば問題ないため。 
  
## ソリューション  
- モデルクオリティのテスト(= **Vertex AI Experiments**)  
テストデータに対して予測を行い、一定の閾値以下なら停止、みたいな仕組みをpipelineに実装する？   

- 学習データのテスト  
パイプライン内でBQテーブルを呼び出す段階でテストを行えるようにするか？  
    - データがない場合や、件数があまりに少ない場合は下流の学習を進ませない（通知込み）
    - **Vertex AI Feature Store**にはデータブレ検出の機能もあるとのこと
- コードのテスト  
????  



# 

