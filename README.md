# interviewReady.io
### Fundamentals
## Types of databases:Graph, TimeSeries and Object
1. `Graph` - Stores data internally as nodes and edges 
           Used to perform graph queries efficiently
           Some relationals databases are used as Graph databases it happens by creating an additional layer on top of the relational database , it enables users to perform graph operations in relational DB 
           its not as efficient as a normal graph databases example neo4j
2. `Time-series Database` - lets say you want to store no_of_sales per second , here it stores records that are part of time-series ,
           aggregate and compress time-staped. 
           few time series databases are written on top of Relational Databases 
           - Its a sequence of Data Points over a time interval 
           -  a model is like ID , Timestamp , Air quality and Temperature its like data coming from a sensor 
           - example : Sensors Data , Weather Data , System Monitoring Data , queries per second , Website activity Data , Stock Prices , 
              - Uniqueness  
                 - Very high write throughput
                 - Regular and irregular writes
                 - Data needs to be highly compressed
            Examples of Time series DB
              - QuestDB
              - Timescale DB
              - Influx DB
              - Amazon Timestream
              - Cassandra (NoSQL)  
3. `Object Oriented Databases`

## What Database should you choose?
- Postgres has high consistency , durability , but in terms of availability it dosnt provide anything off the shelf if a postgres instance goes down than better 
- cassandra is not that highly consistent 
