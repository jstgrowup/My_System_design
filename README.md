# interviewReady.io
## Fundamentals
### Types of databases:Graph, TimeSeries and Object
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

### What Database should you choose?
- Postgres has high consistency , durability , but in terms of availability it dosnt provide anything off the shelf if a postgres instance goes down than better 
- cassandra is not that highly consistent 

## Consistency in Distributed Systems
### What is Data Consistency?
- It is a measure of how up to date the data is inside a distributed system is 
- If you get a stale data or an inconsistent data than we call it a weakly consistent system 
   - Why Consistency is important?
     - Easier to understand
     - User experience (lets say if i like a post and i refresh i should be seeing the updated like count)
     
### Linearizable Consistency?
- This is the highest level of data consistency 
- in this case we have high latency and low availability 
- RAFT is a very intersting concept ![video](https://youtu.be/IujMVjKvWP4)
### Eventual Consistency?
- Here we can send a stale data for a read request lets say you send an email its in your outbox but its not in your sentbox thats a case of Eventual consistency
- For achieving this we can process read and write requests parallely and conurrently
Note that eventual consistency is a very loose guarantee, and is often coupled with stronger guarantees like "read your writes".

"Read Your Writes" consistency: This type of consistency ensures that a client that makes a write request will immediately see the updated value when it makes a read request, even if the update has not yet propagated to all servers. This type of consistency is often used in systems where low latency is more important than consistency.

"Monotonic Read" consistency: This type of consistency ensures that a client will only see values that are as up-to-date or more up-to-date than the values it has previously seen. In other words, once a client has seen a value, it will never see an older value again.
   - Imagine you and your friend have notebooks, and you both write down important information. This type of consistency is like a rule that says, "Once you read something in your notebook, you'll never see something older later."
   - So, if you read a note that says "Meeting at 3 PM," you won't later see a note saying "Meeting at 2 PM." Once you've seen a piece of information, you won't go back in time and see something older than that. It's like your notebook always shows the most recent or equally recent information, and you won't see outdated stuff once you've already seen something newer.

"Monotonic Write" consistency: This type of consistency ensures that a client will only see values that are as up-to-date or older than the values it has previously written. In other words, once a client has written a value, it will never see a newer value from another client.
   - Imagine you and your friend are writing messages on a shared whiteboard. This type of consistency is like a rule that says, "Once you write something on the board, you'll never see a newer message from your friend."
   - So, if you write "Hello" on the board, no matter what your friend writes later, you won't see it. You'll only see your own "Hello" or something older. It's like freezing the board for you after you've written something, and you won't be bothered by your friend's newer messages.
### Causal Consistency?
- [video ref](https://youtu.be/9YTDvPjWLLM)