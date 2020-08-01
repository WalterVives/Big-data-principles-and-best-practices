# BIG DATA 
## Principles and best practices of scalable real-time data systems by Nathan Marz with James Warren

## Chapter 1  A new paradigm for Big Data.


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

## Chapter 2 Data model for Big Data.

#### 1. What is Semantic normalization?
Semantic normalization is the process of reshaping free- form information into a structured form of data.

#### 2. When store unstructured data?
As a rule of thumb, if your algorithm for extracting the data is simple and accurate, like extracting an age from an HTML page, you should store the results of that algorithm. If the algorithm is subject to change, due to improvements or broadening the requirements, store the unstructured form of the data.

#### 3. More information means rawer data?
It’s easy to presume that more data equates to rawer data, but that’s not always the case. Let’s say that Tom is a blogger, and he wants to add his posts to his FaceSpace profile. What exactly should you store once Tom provides the URL of his blog?
Storing the pure text of the blog entries is certainly a possibility. But any phrases in italics, boldface, or large font were deliberately emphasized by Tom and could prove useful in text analysis. For example, you could use this additional information for an index to make FaceSpace searchable. We’d thus argue that the annotated text entries are a rawer form of data than ASCII text strings.
At the other end of the spectrum, you could also store the full HTML of Tom’s blog as your data. While it’s considerably more information in terms of total bytes, the color scheme, stylesheets, and JavaScript code of the site can’t be used to derive any additional information about Tom. They serve only as the container for the contents of the site and shouldn’t be part of your raw data.

#### 4. What is immutable data?
Immutable data may seem like a strange concept if you’re well versed in relational
databases. After all, in the relational database world—and most other databases as
well—update is one of the fundamental operations. But for immutability you don’t 1
update or delete data, you only add more. 

#### 5. What are the principal advantages of immutable data?

**human-fault** 
tolerance is an essential prop- erty of data systems. People will make mistakes, and you must limit the impact of such mistakes and have mechanisms for recovering from them. With a muta- ble data model, a mistake can cause data to be lost, because values are actually overridden in the database. With an immutable data model, no data can be lost. If bad data is written, earlier (good) data units still exist. Fixing the data system is just a matter of deleting the bad data units and recomputing the views built from the master dataset.

**Simplicity**
Mutable data models imply that the data must be indexed in some way so that specific data objects can be retrieved and updated. In contrast, with an immutable data model you only need the ability to append new data units to the master dataset. This doesn’t require an index for your data, which is a huge simplification. As you’ll see in the next chapter, storing a master dataset is as simple as using flat files.

#### 6. In which special cases you have to delete your data?

**Garbage collection** When you perform garbage collection, you delete all data units that have low value. You can use garbage collection to implement data- retention policies that control the growth of the master dataset. For example, you may decide to implement a policy that keeps only one location per person per year instead of the full history of each time a user changes locations.

**Regulations—Government** regulations may require you to purge data from your databases under certain conditions.

## Chapter 3 Data model for Big Data: Illustration.

#### 1. Why a serialization framework?

Many developers go down the path of writing their raw data in a schemaless format like JSON. This is appealing because of how easy it is to get started, but this approach quickly leads to problems. Whether due to bugs or misunderstandings between differ- ent developers, data corruption inevitably occurs. It’s our experience that data cor- ruption errors are some of the most time-consuming to debug.

Data corruption issues are hard to debug because you have very little context on how the corruption occurred. 
Serialization frameworks are an easy approach to making an enforceable schema. If you’ve ever used an object-oriented, statically typed language, using a serialization framework will be immediately familiar. Serialization frameworks generate code for whatever languages you wish to use for reading, writing, and validating objects that match your schema.
However, serialization frameworks are limited when it comes to achieving a fully rigorous schema. After discussing how to apply a serialization framework to the Super- WebAnalytics.com data model, we’ll discuss these limitations and how to work around them.

#### 2. What is Apache Thirft?

Apache Thirft is a tool that can be used to define statically typed, enforceable schemas. It provides an interface definition language to describe the schema in terms of generic data types, and this description can later be used to auto- matically generate the actual implementation in multiple programming languages.

#### 3. What is an edges?

Each edge can be represented as a struct containing two nodes. The name of an edge struct indicates the relationship it represents, and the fields in the edge struct contain the entities involved in the relationship.

#### 4. Limitations of serialization frameworks.
erialization frameworks only check that all required fields are present and are of the expected type. They’re unable to check richer properties like “Ages should be non- negative” or “true-as-of timestamps should not be in the future.” Data not matching these properties would indicate a problem in your system, and you wouldn’t want them written to your master dataset.
This may not seem like a limitation because serialization frameworks seem some- what similar to how schemas work in relational databases. In fact, you may have found relational database schemas a pain to work with and worry that making schemas even stricter would be even more painful. But we urge you not to confuse the incidental complexities of working with relational database schemas with the value of schemas themselves. The difficulties of representing nested objects and doing schema migra- tions with relational databases are non-existent when applying serialization frame- works to represent immutable objects using graph schemas.

## Chapter 4 Data storage on the batch layer.

#### 1. How to determinate the Storage requirements for the master dataset?
To determine the requirements for data storage, you must consider how your data will be written and how it will be read. The role of the batch layer within the Lambda Architecture affects both areas.

#### 2. What is the problem using a key/value store for the master dataset?

The biggest problem, though, is that a key/value store has a lot of things you don’t need: random reads, random writes, and all the machinery behind making those work. In fact, most of the implementation of a key/value store is dedicated to these features you don’t need at all. This means the tool is enormously more complex than it needs to be to meet your requirements, making it much more likely you’ll have a problem with it. Additionally, the key/value store indexes your data and provides unneeded services, which will increase your storage costs and lower your performance when reading and writing data.

#### 3. What is the problem with Distributed filesystems?

The problem with a regular filesystem is that it exists on just a single machine, so you can only scale to the storage limits and processing power of that one machine. But it turns out that there’s a class of technologies called distributed filesystems that is quite similar to the filesystems you’re familiar with, except they spread their storage across a cluster of computers. They scale by adding more machines to the cluster. Distributed filesystems are designed so that you have fault tolerance when a machine goes down, meaning that if you lose one machine, all your files and data will still be accessible.

#### 4. What is Vertical partitioning?

Although the batch layer is built to run functions on the entire dataset, many compu- tations don’t require looking at all the data. For example, you may have a computa- tion that only requires information collected during the past two weeks. The batch storage should allow you to partition your data so that a function only accesses data relevant to its computation. This process is called vertical partitioning, and it can greatly contribute to making the batch layer more efficient. While it’s not strictly necessary for the batch layer, as the batch layer is capable of looking at all the data at once and filtering out what it doesn’t need, vertical partitioning enables large performance gains, so it’s important to know how to use the technique.
Vertically partitioning data on a distributed filesystem can be done by sorting your data into separate folders.

#### 5. What are the main characteristics between XML, CSV and JSON?

In XML and CSV you cannot distinguish between a number and a string consisting of digits.
JSON distinguishes strings and numbers, but does not distinguish integers and floating point numbers, and does not specify a precision.
JSON and XML have good support for Unicode character strings, but do not support binary strings.
There is optional schema support for XML and JSON, and schema languages are quite powerful and, therefore, complicated to learn and implement.
CSV does not have a scheme, so it is up to the application to define the meaning of each row and column. If an application change adds a new row or column, you have to handle that change manually.
