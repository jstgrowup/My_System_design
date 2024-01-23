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
        - At this consistency level, we want to show all changes in the database until the current read request. It means all changes which have happened in the database before the read operation will be reflected in the read query. 

        For example, suppose initially we had x = 10. 

        update x to 13 

        update x to 17 

        read x --> Returns 17

        update x to 1 

        read x --> Returns 1 

       To achieve this we use a single-threaded single server. So every read-and-write request will always be ordered. Using the above example, the first read x will be executed after updating the value to 17. This is useful when systems need perfect consistency. 
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
### Quorum
- [Medium Article](https://medium.com/@sunny_81705/quorum-in-distributed-systems-37cbe17aae88)
- Minimum number of nodes in cluster that must be online and be able to communicate with each other. If any additional node failure occurs beyond this threshold, cluster will stop running.
- QUorum works on some kind of consensus in the distributed system 
- Its also called as evetually consistent system 
-  One piece of data is stored in multiple nodes. So we need some sort of mechanism so that nodes agree on a particular return value.

    Need for quorum
    Let's take an example,

    Suppose we replicate the data on 3 nodes. Node 0,1 and 2.
    We made a written request to node 0. Data is written on Node 0 and Node 1 and 2 are waiting for the data.
    Meanwhile, we have a read request for the newly inserted data but Node 0 fails.
    Since Node 0 fails it checks in Node 1 and 2. But the replicas are still not inserted in these nodes.
    So our database sends a data not found error which is false.
    Instead, the database should have sent database error.
    To avoid such issues we need a distributed consensus and one way to implement that is through Quorum.

    How does quorum work?
    In quorum, if a particular number of nodes (which is called the Quorum factor) agree on a particular value that value is sent back.

    If we have data on multiple nodes then we take the data with the latest timestamp (Timestamp is just one factor. You can choose any factor as per your use case). So even if one of our nodes fails users can still read the data.

    Does quorum guarantee that the return value will always be correct? NO.

    Considering the above example only. Suppose we have a quorum factor of 2. There are 3 participating nodes and 2 nodes agree that data does not exist (which is false).

    If we have a Quorum factor of 3 then the query will fail.

    So, low quorum factor provides more availability at the cost of consistency whereas high quorum factor provides more consistency but makes the system less available.



      #### Summary
      ╔═════════════╦══════════════╦══════════╦═════════╦══════════════╗
      ║             ║ Serializable ║ Eventual ║ Causal  ║ Quorum       ║
      ╠═════════════╬══════════════╬══════════╬═════════╬══════════════╣
      ║ Consistency ║ Highest      ║ Lowest   ║ Mid-way ║ Configurable ║
      ╠═════════════╬══════════════╬══════════╬═════════╬══════════════╣
      ║ Efficiency  ║ Lowest       ║ Highest  ║ Mid-way ║ Configurable ║
      ╚═════════════╩══════════════╩══════════╩═════════╩══════════════╝


### Transaction Isolation Levels - Read Uncommitted Data
- Stages of transaction
      *BEGIN* Marks the start of the transaction.
      *COMMIT* Marks the end of the transaction, which results in the changes persisting to the database.
      *ROLLBACK* Marks the end of the transaction and undoes all the changes to the database.



- It is more specific consistency talks about multiple transactions whereas Trasactions talks about a single transaction
- Begin and commit are an indication for the DB to register that there is transaction happening on it 
- Isolation:
   If two transactions are running concurrently and queries in one transaction do not affect the other transaction then the two transactions are said to be isolated from each other. 
- Transaction isolation levels are a measure of the extent to which transaction isolation succeeds
   *Dirty Reads* :
      Imagine you have two people (transactions), and one person is updating some information.
      The other person checks that information before the first person finishes making changes.
      If the first person decides to cancel the update, the second person ends up reading information that never really got confirmed or saved.
   *Nonrepeatable Reads* :
      Picture a person reading a book (a row in a database).
      Another person comes and changes the content of that book, and then the first person reads it again.
      The problem is, the second time they read it, the information is different or, worse, the book is deleted. It's like reading a book that keeps changing or disappears.
   *Phantoms*:
      Think of a person searching for a specific type of item in a store (a set of rows in a database).
      While this person is looking for items, the store clerk (another transaction) adds new items that fit the search criteria.
      When the first person searches again, they find items that weren't there before. These "new" items are like phantoms – they appeared out of nowhere.
- In summary, these concepts are about problems that can happen when multiple actions (transactions) are taking place at the same time in a database. Dirty reads involve reading unconfirmed changes, nonrepeatable reads involve reading different information over time, and phantoms involve finding things in a search that weren't there before due to new additions. Transaction isolation levels help manage these issues to ensure data consistency and reliability in a database.
- here lets say we have a like count and we are reading the uncommited transactions as well in that case we have higher level of isolation for lower level of efficiency

### Read Commited
- At this isolation level, one transaction can only read committed data from other transactions.
- An example would be a movie theater booking system . Reading committed data would be appropriate while booking seats since we ensure that a seat is only counted as booked once the transaction has been committed
- [video](https://youtu.be/-gxyut1VLcs)
### Repeatable Commited
- 
### Serializable Isolation Level
- This is the highest level of isolation 
- At this level all transactions are executed serially
- All of the operations are executed serially or every operation is ensured to not meddle with the operations of other transactions
- we use this isolation level when we want to avoid Phantom reads
- A phantom read occurs when two identical read queries are executed and the collection of rows returned by the second query is different from the first . It is different from non-repeatable reads because in phantom reads the change in the value can be due to the insertion of a new row 
### Conclusion Transaction Isolation
- ![Image](https://github.com/jstgrowup/My_System_design/assets/40628582/24ad653f-1129-4df8-96b2-0040e9b351d0)
- [Summary](https://d3ivphvzw9pbeo.cloudfront.net/consistency-in-distributed-system.pdf?Expires=1706065998&Signature=QfEMMQM1vMbJySJBs4DzM03rWv7w2qweqsUSSYzdwbWeS6cxfNxQE4fRWC3Tc3x~0g3JKfGfEY0~cd9aNNIrsgm4auZ9KwGAzRR8wE7q44aOIgGNfNJnL1Q~epsIo~~-jWgiMM1CwE8PR9szJDRz0Rm0kze7WsXWRC9HR2I6zFT5jKh3ZezOq0iWvG2JPqwawEYRLnP51fZ3nFTy50sNVbXaDBR-bozRB9PwWxE5hIM330fqegjTnGRQtxBo1ntLEJogv1Dn8u5Yia0NdXvbNyW~pO5x0Mmo9ra8vxwXTaMa2hshukShNFmRPqmv7wO5LZmGx9S4d2DzuS75LbAfcw__&Key-Pair-Id=K2UERR4YIMY56O)

## Caches Deep Dive
### Caching: Basics
*Benefits oF a cache*
     1. Saves network calls
     2. Avoids repeated conputaions
     3. Reduces DB load
*Drawbacks of a cache*
     1. Can be expensive (cache reads from SSD)
     2. Potential thrashing is like loading the data into the cache 
*Cache policies*
       - cache performance depends on the Cache policy its like a rule for evicting as well as put the data into the cache
       1. LRU(Least recently Used) 
       - whenever you have to put the data into the cache you put the recent ones like seconds ago
       - when you want to evict the data from the cache you delete from the last or from the end the old ones
       - celebrity comments example
       2. LFU(least frequently Used)

If I have a poor eviction policy than what happens  is i make a unncessary call for the cache which is not even there than i go the the DB to get the data.

*`consistency in a distributed system`* : so lets say there is a user profile and i have two servers so he changes the password through server 2 but whil e getting he got the data from the server 1 cache which is outdated

### Caching: Global vs. Local Cache
