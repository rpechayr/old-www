---
layout: post
title: MongoDB inventory control using two-phase commit
category: blog
---

#MongoDB inventory control using two-phase commit

*This is the first post from a series around two-phase commit in MongoDB*

##MongoDB
MongoDB is a No-SQL database engine that has been entirely designed to scale. Its main features are :
- **No Schema**: no database migrations
- **BSON-based documents**: can hold complex structures if necessary
- **Geo Indexing**: documents can have gespatial coordinates and MongoDB can easily find nearest documents from a specific location. When using PostgreSQL for example, you need to load [an additional component](http://postgis.net/) to do that
- **Replica sets** : data can be replicated in real time accros many servers (up to 25). This is interesting to speed up reading (writes only happens on the *primary* node), but the whole magic is when you need to upgrade MongoDB, or when there is a database server failure. It can happen on a Sunday morning and your applications just keep working. **Failover is entirely handled by MongoDB**.
- **Sharded Cluster** : allows users to shard data across multiple servers, thus achieving parallel writes. Sharding can be combined with replica sets if necessary.

The main limitations of MongoDB are:
- **No ACID transactions** like in SQL systems. The only true transactions are per document in MongoBD, you can't have isolated transactions like in other DBMS.
- **No joins** : it is not possible to build complex queries accross multiple collections (equivalent of tables in MongoDB)
- **Index limitations** : MongoDB indexing technologies is less mature than what has been done for decades in SQL databses. Furthermore, search engine technologies like Elastic Search provide advanced search features along with modern and size-optimized indexing algorithms.


This allows mongo to be able to store the same collection of objects in 20 servers if necessary using sharded clusters. Every requests will be forwarded to the right server to be able to handle many writes in paralell. 

##Two-phase commit

Two-phase commit is a classical pattern used to implement 
transactions, thre is a great example


##The problem
Suppose you're working on an e-commerce app, where you not only handle sales but also inventory. You typically won't store the sales information in the same place as the number of remaining items in your Database. An important part of the process though is to make sure a product is still available whenever someone wants to buy it. you also want to make sure that if only 1 unit is remaining, only 1 person can buy it. 
