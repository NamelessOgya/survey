# Google Cloud Big Data and Machine Learning Fundamentals  
  
# Week1  
reading list  
## intro  
Three layers of google infra stracture
 1. network & security(base)
 2. compute and Storage
 3. Big data and ML infra

network & security layer is out of scoop of this course  
  
## Compute  
- products for computation
  - compute engine  
      - laaS product(?)
      - similar to physical data centers
        - compute
        - storage
        - network
  - Google Kubernetes Engine(GKE)  
    - runs contenarized app in a cloud env
  - App Engine  
    - Fully managed PaaS offering(?)
  - Cloud functions  
    - Excute code in responce to events
    - Function as service(?)
  - Cloud run  
    - fullly managed compute platform
    - run request or event-driven stateless workload
- example  
  - google cloufd video stabilization
    - deploy smaller trained version of the model to smartphone
    - user cloud TPU for faster training
## Strage  
![image](https://user-images.githubusercontent.com/54636129/219830646-eb27cf29-9d1f-41c9-b8a9-36160de55237.png)  
- unstructured data
  - cloud strage
- structured data
  - transactional  
  online transaction processing system  
  first data update and insert is rquired
    - SQL  
      - cloud SQL
      - Cloud Spanner
    - No SQL
      - Firestore
  - Analytical
    - SQL
      - BQ
    - No SQL
      - milisecond latency

## big data and ML products  
...

## Big data and ML product categories  
Data-to-AI workflow  
- ingestion & process  
for data ingestion for realtime/batch data  
    - Pub/Sub  
    - Dataflow
    - Dataproc
    - cloud data fusion
- Storage  
  - cloud strage
  - cloud sql
  - cloud spanner
  - big table
  - fire store
- Analytics  
  - BQ
  - Data studio
  - Looker
- Machine Learning
  - Vertex AI
  - AutoML
  - Workbench
  - tf
  - Document AI
  - Contact center
  - Retail Product Discovery
  - Health care data engine

# reading  
元リスト[■](https://d3c33hcgiwev3.cloudfront.net/XZrUMfdWTjGa1DH3Vl4x7A_c999fb9bd6f94d8db2719dc7c59b54a1_M1-_-Reading-list-_-Big-Data-and-Machine-Learning-Fundamentals.pdf?Expires=1676851200&Signature=CJmcCRTXyrGGiVO0uBz9c8qlG7yEtwvlfCRggvnmwsJY8H-DXRRqOJY-KVA3IIB~5O7T0JIyXgC4O3fUp04Hbsssj9sgjtnj5-ZE5W5mBl1dp3Um7I8GxVvlKVJgnGZtwE~Od51m4uMQjMHjIz2sV9BzYJ2DUVYNdsgvFQ7TULM_&Key-Pair-Id=APKAJLTNE6QMUY6HBC5A)  
  
特に弱い部分を抜粋↓  
[TPU関連](https://storage.googleapis.com/nexttpu/index.html)  
[TPUの動作原理](https://storage.googleapis.com/nexttpu/index.html)  
[Google cloudのチートシート](https://googlecloudcheatsheet.withgoogle.com/)  
[Google ML product](https://cloud.google.com/products?hl=ja#section-3)  



