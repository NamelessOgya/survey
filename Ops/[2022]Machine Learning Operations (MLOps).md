# [2022]Machine Learning Operations (MLOps):Overview, Definition, and Architecture  
論文調査や有識者へのヒアリングを通して、MLOpsがについてまとめた論文
# Results  
## Principles  
![image](https://user-images.githubusercontent.com/54636129/203879535-19793766-8c2e-42a2-bc9a-44cf173d5dfe.png)  
MLOpsで実現しなければならないPrincipleは以下の9つである。  
- P1 CI/CD Automation  
  - continuous integration
  - continuous deliverly
  - continuous deployment
- P2 Workflow orchestration  
  - タスクをDAGにしたがって実行する。
- P3 Reproducibility(再現性)  
  - 実験を再現することができ、同じ結果が得られること
- P4 Versioning  
  - データ/モデル/コードのバージョンを管理すること
  - 再現性担保と追跡性（コンプライアンスや原因究明）のために使用。
- P5 Collaboration  
  - データやモデル・コードに対して協調して働けること
  - テクニカルな側面でも、カルチャーの側面でも（部門の壁を超える、など）
- P6 Continuous ML training & evaluation  
   -  新たなデータを用いた定期的なre-training
   -  コンポーネントのモニタリング / FBループ / 自動化されたトレーニングパイプライン　によって実現される。
   -  モデル品質の評価もセットで行う。

- P7 ML metadata tracking/logging  
  - モデルのパラメーター  
  - イテレーションごとの結果
  - 学習結果
  - 使用されたコード  
  - モデルの「血統」

- P8 Continuous monitoring  
  - 定期的なモデル、コード、インフラの評価
  - 品質の低下や潜在的なエラーを検出する。  

- P9 Feedback loops  
  - デプロイやテスト段階のフィードバック
  - コンポーネントモニタリングのフィードバック

## Technical Components  
- C1 CI/CD Component (P1, P6, P9)
  - 要件  
    - CI/CDができること
    - ビルド/テスト/デプロイ/デリバリができること
    - 各ステップに関してのフィードバックをデベロッパーに行えること
  - 使われるサービス
    - Jenkins  
    - GitHub actions  

- C2 Source Code Repository (P4, P5)  
  - 要件
    - 複数人がコードをアップロードして、バージョン管理と保存ができることが必要  
  - 使われるサービス  
    - Bitbucket  
    - GitLab
    - GitHub
    - Gitea
- C3 Workflow Orchestration Component (P2, P3, P6)  
  - 要件  
    - ワークフローをDAGで表現して、実行できること
  - 使われるシステム
    - Apache Airflow
    - Kubeflow Pipelines  
    - Luigi
    -  AWS SageMaker Pipelines
    -  Azure Pipelines
- C4 Feature Store System (P3, P4)
  - 使われるデータを保存しておく場所。
    - offline feature store  
      - 実験用に、通常のレイテンシのデータをおいておく場所
    - online feature store
      - 実製品でのproductionようにレイテンシの少ないデータを持っておく場所
  - 使われるサービス  
    - Google Feast  
    - Amazon AWS Feature Store  
     
- C5 Model Training Infrastructure (P6)  
- C6 Model Registry (P3, P4)
  - Model artifactとメタデータを同時に保管する。
  - 使われるサービス
    - AWS SageMaker Model Registry
    - Microsoft Azure ML Model Registry
    - 別にGCSなどのシンプルなストレージでもOK
- C7 ML Metadata Stores (P4, P7)  
    - トレーニングjobのイテレーションごとの結果などより詳細なMetadataはここに保存する。  
- C8 Model Serving Component (P1).  
  - 実際にモデルをAPIとして提供する部分  
  - 実際に使われるサービス
    - Microsoft Azure ML REST API
    - AWS SageMaker Endpoints
    - IBM Watson Studio
    -  Google Vertex AI prediction service 

- C9 Monitoring Component (P8, P9)  
  - モデルのパフォーマンスを監視する  
  - kubeflowなど


## Roles  
興味ないので図のみ  
![image](https://user-images.githubusercontent.com/54636129/203884542-bd860893-1cd2-4de7-9062-7c9ce2b6d4c1.png)  

## Architecture and Workflow  
Bのデータ成型の部分はデータエンジニアの仕事。
DevOpsエンジニアがかかわるのは主にDの部分     
![image](https://user-images.githubusercontent.com/54636129/203884894-994eb9cd-ee48-4375-9743-ade3523dfa28.png)  

## Conceptialization(MLOpsとは)  
- MLOpsはパラダイムである。
- 機械学習とDevOps(ソフトウェアエンジニアリング)、データエンジニアリングを活用する実践工学である。
- MLOpsは以下のprincipleを利用することで、機械学習プロダクトを作りやすくすることを狙いとしている。
  - CI/CD automation
  - workflow orchestration
  - reproducibility 
  - versioning of data, model, and code
  - collaboration
  - continuous ML training and evaluation
  - ML　metadata tracking and logging
  - continuous monitoring
  - feedback loops.

