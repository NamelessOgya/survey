# Google Cloud Big Data and Machine Learning Fundamentals  
  
# Week1  data engineering for streaming data  
## intro  
- data engineering for streaming data  
  - ingest data
    - pub_sub
  - process the data
    - data flow
  - visualize
    - data studio
    - looker
## big data challenges  
- 4Vs  
  - variety  
    - data may come in various format
  - volume
    - peta-bite sacale data may come suddenly
    - scalability is needed
  - velocity
    - data should be processed in nearly real time
  - varacity
    - maintain the quality of data
      - insonsistency
      - uncertanity

## Message-oriented architecture  
- Pubsub
  - distributed messaging service
  - distribute various kind of data(topic) to corresponding subscrivers

## Designing streaming pipelines with Apache Beam  
- Dataflow  
  - excusion engine for apatche beam
  - serverlenn and NoOps
    - No mentenance is needed. these are automated
      - Mentenance
      - Monitoring
      - Scaling
    - these are done on behalf of the user
      - resourse provisioning
      - performance tuning
      - pipeline reliability
  - dataflow template
    - streaming template
    - batch template
    - utility template

## Visualization with Looker  
## Visualization with Data Studio
## demo
- make dataflow pipeline to connect pubsub and BQ
  - dataflow pipeline is very easy to use
## documentation  
[PubSub documentaion](https://cloud.google.com/pubsub/docs?hl=ja)  
[data flow documentaion](https://cloud.google.com/dataflow/docs?hl=ja)  
[data flow template](https://cloud.google.com/dataflow/docs/concepts/dataflow-templates?hl=ja)  

