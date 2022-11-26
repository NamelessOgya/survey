# MLOps役立ちurls
## サービス公式ドキュメント  
- Vertex AI pipelines  
  - 概要    
    https://cloud.google.com/vertex-ai/docs/pipelines/introduction?hl=ja
- kubeflow pipelines SDK  
https://www.kubeflow.org/docs/components/pipelines/v1/sdk/sdk-overview/
- tutorial
https://colab.research.google.com/github/GoogleCloudPlatform/vertex-ai-samples/blob/main/notebooks/official/pipelines/lightweight_functions_component_io_kfp.ipynb?hl=ja#scrollTo=title%3Ageneric  

- GCSをクライアントから操作するblob  
https://cloud.google.com/python/docs/reference/storage/latest/google.cloud.storage.blob.Blob

## やってみた話
Kubeflow PipelinesからVertex Pipelinesへの移行による運用コスト削減  
https://techblog.zozo.com/entry/migrate-to-vertex-pipelines

## その他  
kfp_v2ではfunc_to_container_op/create_component_from_funcの使用は非推奨で  
いずれ@componentの使用に一本化される。  
https://github.com/kubeflow/pipelines/issues/7794  
  
