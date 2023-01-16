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
  
## 疑問  
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
  
# C2 Source Code Repository (P4, P5)    
> The source code
repository ensures code storing and versioning. It allows multiple
developers to commit and merge their code [17,25,42,44,46] [α, β,
γ, ζ, θ]. Examples include Bitbucket [11] [ζ], GitLab [11,17] [ζ],
GitHub [25] [ζ ,η], and Gitea [46].
- コードのストアとバージョニングを行う。


## ソリューション  
Git hubなど？Googleには当該サービスはないかも。

# C3 Workflow Orchestration Component (P2, P3, P6)  
>The
workflow orchestration component offers task orchestration of an
ML workflow via directed acyclic graphs (DAGs). These graphs
represent execution order and artifact usage of single steps of the
workflow [26,32,35,40,41,46] [α, β, γ, δ, ε, ζ, η]. Examples include
Apache Airflow [α, ζ], Kubeflow Pipelines [ζ], Luigi [ζ], AWS
SageMaker Pipelines [β], and Azure Pipelines [ε].
  
## Solution 
**Vertex AI pipelines**  
  
# C4 Feature Store System (P3, P4)  
> A feature store system
ensures central storage of commonly used features. It has two
databases configured: One database as an offline feature store to
serve features with normal latency for experimentation, and one
database as an online store to serve features with low latency for
predictions in production [10,14] [α, β, ζ, ε, θ]. Examples include
Google Feast [ζ], Amazon AWS Feature Store [β, ζ], Tecton.ai and
Hopswork.ai [ζ]. This is where most of the data for training ML
models will come from. Moreover, data can also come directly
from any kind of data store.
- 実験用の通常レイテンシのデータベースと予測用の低レイテンシのデータベースが必要  
  - モデルを製品として導入する際には必要なのかも？
- 
## 疑問  
通常のデータベースではなく、Feature Storeを使うのはなぜか。  
⇒　（調査中）

## ソリューション  
**Vetex AI Feature Store**[リンク](https://cloud.google.com/vertex-ai/docs/featurestore/overview?hl=ja)  
(調査中)
  
# C5 Model Training Infrastructure (P6)  
>. The model training
infrastructure provides the foundational computation resources,
e.g., CPUs, RAM, and GPUs. The provided infrastructure can be
either distributed or non-distributed. In general, a scalable and
distributed infrastructure is recommended [7,10,24–
26,29,40,45,46] [δ, ζ, η, θ]. Examples include local machines (not
scalable) or cloud computation [7] [η, θ], as well as non-distributed
or distributed computation (several worker nodes) [25,27].
Frameworks supporting computation are Kubernetes [η, θ] and Red
Hat OpenShift [γ].  
- モデルトレーニングのための基礎的なcomputation環境(CPU / RAM / GPU)を提供する  
- スケータブルで分散可能な環境が推奨される。  

## ソリューション  
**Vertex AI pipelines**  
  
# C6 Model Registry (P3, P4)  
> The model registry stores
centrally the trained ML models together with their metadata. It has
two main functionalities: storing the ML artifact and storing the ML
metadata (see C7) [4,6,14,17,26,27] [α, β, γ, ε, ζ, η, θ]. Advanced
storage examples include MLflow [α, η, ζ], AWS SageMaker
Model Registry [ζ], Microsoft Azure ML Model Registry [ζ], and
Neptune.ai [α]. Simple storage examples include Microsoft Azure
Storage, Google Cloud Storage, and Amazon AWS S3 [17]
- メタデータとともにモデルをstoreしておく場所  
  
## ソリューション  
**Vertex AI Model Registory**  
**Vertex AI Metadata Store**  
(調査中)  

#  C7 ML Metadata Stores (P4, P7)  
> ML metadata stores allow
for the tracking of various kinds of metadata, e.g., for each
orchestrated ML workflow pipeline task. Another metadata store
can be configured within the model registry for tracking and
logging the metadata of each training job (e.g., training date and
time, duration, etc.), including the model specific metadata—e.g.,
used parameters and the resulting performance metrics, model
lineage: data and code used [14,25–27,32] [α, β, δ, ζ, θ]. Examples
include orchestrators with built-in metadata stores tracking each
step of experiment pipelines [α] such as Kubeflow Pipelines [α,ζ],
AWS SageMaker Pipelines [α,ζ], Azure ML, and IBM Watson
Studio [γ]. MLflow provides an advanced metadata store in
combination with the model registry [32,35].  
- pipeline taskごとに様々な種類のメタデータを保存できる。  
  - トレーニングjobの結果/ パラメータ/ 使われたコード/ モデルの系統をログとしてモデルレジストリに紐づけて保存する。

## ソリューション  
**Vertex AI Metadata Store**  
(調査中)

# C8 Model Serving Component (P1)
> The model serving
component can be configured for different purposes. Examples are
online inference for real-time predictions or batch inference for
predictions using large volumes of input data. The serving can be
provided, e.g., via a REST API. As a foundational infrastructure
layer, a scalable and distributed model serving infrastructure is
recommended [7,11,25,40,45,46] [α, β, δ, ζ, η, θ]. One example of
a model serving component configuration is the use of Kubernetes
and Docker technology to containerize the ML model, and
leveraging a Python web application framework like Flask [17]
with an API for serving [α]. Other Kubernetes supported
frameworks are KServing of Kubeflow [α], TensorFlow Serving,
and Seldion.io serving [40]. Inferencing could also be realized with
Apache Spark for batch predictions [θ]. Examples of cloud services
include Microsoft Azure ML REST API [ε], AWS SageMaker
Endpoints [α, β], IBM Watson Studio [γ], and Google Vertex AI
prediction service [δ].
- モデルを提供するためのコンポーネントである（REST APIなど）

## ソリューション  
**Vertex AI Prediction**(エンドポイント？)
(調査中)  
  
# C9 Monitoring Component (P8, P9).  
> The monitoring
component takes care of the continuous monitoring of the model
serving performance (e.g., prediction accuracy). Additionally,
monitoring of the ML infrastructure, CI/CD, and orchestration are
required [7,10,17,26,29,36,46] [α, ζ, η, θ]. Examples include
Prometheus with Grafana [η, ζ], ELK stack (Elasticsearch,
Logstash, and Kibana) [α, η, ζ], and simply TensorBoard [θ].
Examples with built-in monitoring capabilities are Kubeflow [θ],
MLflow [η], and AWS SageMaker model monitor or cloud watch
[ζ].
  
## 疑問  
CI / CDのモニタリングってなに？　　　

## ソリューション 
**Vertex AI Prediction**(継続的モニタリング)  
モデルの劣化をモニタリングすることが可能

