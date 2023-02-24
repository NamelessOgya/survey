# intro  
# ML on google cloud  
## What is ML  
## WHat kind of problem can it solve  
Google thinks of ML as the way to scale, to automate, to personalize  
- instead of apply rule, add data for it  
## Activity intro Framing a machine learning problem  
machine learning frame  
- ML problem  
  - what is being predicted  
  - what data do we need  
- AS a software problem  
  - who use this service  
  - how are they doing it today  
- AS a data problem  
  - what date we need to collect
  - what .. analyze  
  - what data are we reacting to  
  reaction to predicted result  

## Infuse your apps with ML  
take advantage of pre-trained model  
pre trained aip is useful  
  
## Build a data strategy around ML  
- rules and models  
    - data wins every time
    - you should spend your time collecting your data  
      - ML strategy is data strategy

- go through manual data analysis to ML is reccommended  
  - collecting data is often the longest and hardest part of ML  
  - manual analysis help you fail first and try new ideas
  - to build ML model, you have to know your data
  - ML is a journey towards automation and scale of manual analysis

- trraining-serving skew  
  - if the model doesnt see exact same data in training step, 
  model quality become low because of it.
  - to reduce this, we should use same code to process training data and serving data  
    - your processing should do batch and streaming preprocess
      - dataflow is suitable
        - apatch beam can process streaming and batch data
          - b...batch
          - eam...stream
- performance metrics are different
  - training
    - scaling to alot of data
  - serving
    - high qps  
  - tensorflow is suitable not only for training but also operationalization  

# How Google does ML  
## Introduction  
## ML surprise   
- ML effort allocation  
  - category  
    - Defining kpis
    - callectiong data
    - building infra
    - optimizing ML
    - Integration
  - effort allocation  
    -  optimizing ML takes less effort than expected
    -  pay more attention to data collection / infra
## The secret sause  
- top 10 ML pitfalls  
  - ML require just as much software infra  
    - someone may assume ML is easy to implement...  
    but usually need as much infra as normal software  
  - No data clolected yet
  - Assume the data is ready for use  
  - keep human in the roop  
    - we must keep human in the roop to keep quality high
  - product launch focused on thw ML algorithm  
  - ML optimizing for the wrong thing
  - Is your ML improving things in the real world  
    - monitoring system is needed  
  - Using a pre-trained ML algorithm vs building your own  
    - pre-trained API is really easy to use.  
  - ML algorithms are trained more than once  
    - if your model get good score in one training set, it is not enough.
  - trying to design your own perception or NLP algorithm
    - sometimes it will be waste of time
- good news  
  - most ML value comes along the way  
  - ML improves almost everything it touches  
  - if ML is hard, it is hard for your competitors too
  - ML is a great differentiator

## ML and busness process  
ordinal business process has feedback roop, which continuously refine the quality of operation   
  
## the path to ML  

## A closer look at the path  
1. indivisual contributor  
   - dangers of skipping this stage  
     - inability to scale
     - product head make big, incorrect assumptions that are hard to change later  
   - dangers of lingering too long here  
     - one person get skilled and then leaves  
     - fail to scale up the process to meet demand in time
2. delogation  
    - danger of slipping this step  
      - Not forced to formalize the process
      - Inherent diversity in human responces become a testbed
        - great product learning opportunity
          - chance to collect diverse data
      - Great ML systems will need humans in the roop
   -  dangers of lingering too long here  
      -  pay high marginal cost
      -  more voice say automation isnt possible
      -  organizational lock in
3. 

