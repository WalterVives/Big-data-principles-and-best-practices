# BIG DATA 
## Principles and best practices of scalable real-time data systems by Nathan Marz with James Warren

##### In this document you will find the most relevant questions of the chapter 1 and 2 of the book.


##### 1. Why NoSQL is the solution for all the problems?  
NoSQL databases like Cassandra achieve their scalability by offering you a much more limited data model than you’re used to with something like SQL. Squeezing your application into these limited data models can be very complex. And because the databases are mutable, they’re not human-fault tolerant.

##### 2. If you have a web page and you have a lot of requests is a good option use scaling by sharding the database? 
No, because as the application gets more and more popular, you keep having to reshard the database into more shards to keep up with the write load. Each time gets more and more painful because there’s so much more work to coordinate. And you can’t just run one script to do the resharding, as that would be too slow. You have to do all the resharding in parallel and manage many active worker scripts at once. You forget to update the application code with the new number of shards, and it causes many of the increments to be written to the wrong shards. So you have to write a one-off script to manually go through the data and move whatever was misplaced.


##### 3. How will Big Data techniques help?
The Big Data techniques you’re going to learn will address these scalability and com- plexity issues in a dramatic fashion. First of all, the databases and computation systems you use for Big Data are aware of their distributed nature. So things like sharding and replication are handled for you. You’ll never get into a situation where you acciden- tally query the wrong shard, because that logic is internalized in the database. When it comes to scaling, you’ll just add nodes, and the systems will automatically rebalance onto the new nodes.

##### 4. What does a data system do?
A data system answers questions based on information that was acquired in the past up to the present.

##### 5. What happend if you build immutability and recomputation into the core of a Big Data system?  
The system will be innately resilient to human error by providing a clear and simple mechanism for recovery.

##### 6. What is scalability and lambda architecture is scalable?
Scalability is the ability to maintain performance in the face of increasing data or load by adding resources to the system. The Lambda Architecture is horizontally scalable across all layers of the system stack: scaling is accomplished by adding more machines.
