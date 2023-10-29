# References
1. https://pub.towardsai.net/exploring-the-nosql-family-49e9f23313ad
2. https://aws.amazon.com/nosql/key-value/
3. https://medium.com/helpdotcom/techtalk-tackling-flexible-schema-in-relational-databases-ddcf0c095615
4. https://inviqa.com/blog/understanding-eav-data-model-and-when-use-it
5. https://www.mongodb.com/compare/cassandra-vs-mongodb
6. http://dbmsmusings.blogspot.com/2010/03/distinguishing-two-major-types-of_29.html
7. https://www.pythian.com/blog/cassandra-use-cases

<br />

# MongoDB versus sycalla db
https://www.youtube.com/watch?v=ch1vRQBiXtI&t=8s
Favours flexibility over performance
Sycalla favours consistent perforamce
1. Feature set as evolved over time:
   1. WiredTiger Engine based data storage (3.2)
   2. change data streams
   3. ability to apply schema to collection (3.6)
   4. distributed acid transaction support
2. Core -
   1. C++
   2. XFS filesystem
   3. unbounded cores for mongodb processing
   4. field level encryption support, 
3. Cluster topology
   1. replica set has single primary node for writes
   2. in case of sharding - multiple replica sets gets created and coordinator, router nodes
   3. for sharding to work - background process continously moves data/chunks, this workload and business access patterns
      workload are not bounded to any cpu cores and thus have big performance impact.
4. Ops perspetive
   1. Sharding cluster takeaways mgmt. -  TCO and Ops rating are ba
   2. Single replica set based topology with vertical scaling master for writes is not bad(another way could be to migrate to sycalla db)
5. Dev perspective
   1. Geospatial queries
   2. text search
   3. aggregation pipelines
   4. graph queries
   5. change streams
6. Strengths of Mongodb
   1. support query over flexible schemas like webtracking data
   2. 
   
start with the traditional Who am I so I'm Alexi I'm CTO at number Li nobody is
a marketing technologist that he's helping brands create a digital
relationship with their customers and prospects through usage of data I've
placed a lot of hints in this slide so that you can pinpoint about where I live
in case my accent already tell you that as Greg said I'm a Gentoo Linux
developer I've been in this field for light more than eight years now where I
have the pleasure of packaging and looking into the source code of things like MongoDB Redis and of course Scylla
I also package and have special interests in cluster related
technologies and there are Python bindings so I'm an open source and 2zs
and contributor have been contributed to to salon I have some to pr's open
actually at the moment so please guys that was my cue and like the right time
I guess and I'm a PSF contributing member which means that I spent a fair
amount of my time contributing to various puratone related projects around the world and when I'm at home I'm
teaching my kid to love Scylla as well and you can see she is really supporting the
technology and all the low level design decisions behind it so thank you guys
let's start with a bit of history because it's important to know where
History
those particular technologies come from so it's it's already been a decade since
Cassandra got open source by a by by Facebook and the year later MongoDB got
open source as a GPL license at that time the company behind it was called
engine then MongoDB reached one year
later in 2010 the one point for which was the version that was known as
production ready and it's at the exact same time that number Lea started adopting in production MongoDB so that
tells you that our risk management and early adoption motto is very strong at number Li so that's how we we get into
England - all those technologies then we got the release cycle going on between
Cassandra and and MongoDB each reached reaching and milestones over the years
and then in 2015 Cielo cut open sourced
and as a GPL licensed Cassandra reached 3.0 MongoDB as well then three point to
where they switched their database engine so we're how the store data on disk to
wire tiger on three point two so 2015 has been quite a quite nice year
actually then one year later still I got one point owed and the year after 2.0
2.0 was a counter production rate if I remember correctly and while MongoDB
reached three point six which had two interesting things one is change data
streams and the second one was about having the ability to apply a schema to
to collections and at this time we started looking into into Silla as well
because of some kind of workloads that were a pain for us 2018 MongoDB reached
a new and important milestone with 4.0 and they also changed which is not a
nice milestone in my opinion and door as blogged about it and I encourage you to
read is an insightful opinion on this so
they switch to a SSP l license I felt it was worth mentioning and 2019 MongoDB
got 4.2 with distributed acid transaction through a cluster and Cedar
Ridge 3.0 as you can see in between these slides being made and now we have
three point one as well so three point two was like materialized view production ready and secondary indexes
as well and three point one you already heard about it so it's it's a nice
progress now let's see there are architectural differences and and it
will be more talking to operation side of those technologies the first one is
the core they are both written in C++ so that's something that they share and they have in common and they are using
and advocating for the XFS file system as well that's something that that they
share too and they both have a compaction command that handles low-level and storage
related stuff even if it does not mean exactly the same thing it's about D space and then the most
important difference is how they under and pinpoint some workloads two CPUs
MongoDB does not so you have an unbound course that means that all the
performance and all the things that MongoDB and the engine does is not tied
to specific CPUs while the threat work architecture of Silla is is doing this
so you have been hearing a bit a lot of it since the beginning of the
presentations and the conference that's the word I was looking for and I guess you will be hearing a lot more about it
afterwards security wise that both report on the wire encryption and
encryption at rest and stuff like this they both have the same kind of stuff on
enterprise versions with LGA px cetera for now is still a bit more mature
on the on this on this side it has and supports a field encryption that is
automated on a MongoDB enterprise's you can actually encrypt one one field on
the fly for european-based companies that are running on the GPR
this is something that is sometimes interesting to have now if we go back
Replica Sets
and we go into the architecture in in a purely formal way we will be comparing a
pyramid and I'm not saying triangle for for purpose so a pyramid with a circle
the terminology between this is that this pyramid will be called a replica
set so we will be putting nodes around this this this pyramid on the repeat and
I will form what we call a replica set in MongoDB while in Scylla we will put
nodes around the circle that will form a cluster at the top of this pyramid
leaves a special node the brand Grand Master slashing the slaves all day long
no not really it's called it's more politically correct it's called the
primary node which still has some superpowers compared to the others so at
the bottom of the pyramid and there is the secondary nodes that are living an
old form the replica set it's interesting to note that the data is replicated on
all the older notes so all the data lives on all the notes but one of them
is responsible and is a warranty of this data that is coming and that's the
primary note I'm sorry if my my tongue slips sometimes and to master but
because at some point it was cold like this but it is the primary that I'm talking about and secondaries on the
Silla world it's more like aligned with human rights
I guess because all the nodes are born equal and they all share the the
workload in the same manner so there's nothing special about a node in in the
silica Leicester there are all equals and they are all responsible for a slice of the data set the amount of the data
set that they are responsible for is actually tunable directly through the by
T space have to read what you already heard about the replication factor okay
if you will relate this kind of topologies to the cap theorem what we
see that this primary node has one major
application is to be the warranty of what is coming inside the cluster all
the replica set because it's not yet a cluster will get there that means that in the cap theorem MongoDB favours
consistency and the primary node is the only one of the nodes in the replica set
that has the ability to write that means that only of right Yola rights and right
throughput will go to only one node of your replica set how many secondaries
you will have doesn't care the only thing you need to keep in mind is all
your rights we will go to only one node in a parrot parrot prick a set so the
cylinder is there are they are read-only on the on the silicide though all nodes
are equals so they are all capable of reading and writing data and since they share they have their own
share of this data their own share of burden let's say and you have we will
get a distributed amount of workload between the notes so what's interesting is based on your
replication factor of course it interestingly in from the core you can
see that the design of the distributed hash table consistent hashing based circle of the silic luster and will help
you really balance the the data and make it more available at least it's tunable
thanks to the replication factor okay so now if I look at this and I see the
MongoDB replica set how do i scale those right you can scale the rights by adding
a new primary node right if I add a new primary node I will be heading actually
a new replica set because I can't have multiple primaries per replica set so if
I want to scale this I will have to create a new replica set which means
that at some point I have to split my data set between those replica sets this
is called sharding and you will have to select a special kid a bit like the partition key on the Scylla well that
will be responsible for this and this means that there is there must be a
place where leaves the information of what slice of data lives on what replica
set for this you will have to create a replica set this which is a blue one and
that will hold this very information this configuration or metadata
configuration replica set but then your clients need to know as well when they
connect to what is soon becoming a MongoDB cluster they have to know which
node to address and they come know by themselves so you need
to add and that's the the little rectangles over there you need to have a
new piece in Europe in your topology which are called the s MongoDB
Reuters there are smart Reuters that are connected to the config replicas set so they know where data lives on which
replica set it is and that will route the queries so you can think of them like the coordinator of a club of a
silica Lester or Castlerock Rasta but it's still a new piece that you have to
deploy and maintain and when you have done all of this work congratulations you just created a MongoDB cluster on
the silicide if you want to add more throughput you just add nodes and a
story sorry it's shortcoming I know but that's how it is anyway that's good on
one data center now what if I want to spread this or scale it or stretch it to
multiple regions and multiple data centers this is doable on both worlds of
course I will just have to have a replica set in different regions
I can fairly scale my configuration replica set between the regions as well
every time I will have local s instances to route my queries locally
and it should work pretty well MongoDB has this concept of sharding
zones that also allows you to make sure that I don't know Chinese data stays in
in the asian data center on on the silicide its built-in so it's something
it's there's nothing really really special about it except the latency between data centers and the best route
for every distributed system anyway so the replication factor applied to a
replication strategy we will give you the data location of what you are
looking for inside your now stretched cluster and multi regional cluster
all the rest is done behind the scene for you so let's say look at some
Takeaways
takeaways on the operational side by everything that I told you I guess that
the first thing you need to remember is avoid creating MongoDB cluster please I
could write a book on this very topic on war stories the main reason why apart
from the complexity and we'll get that on TCO and operating just on the next
slide is the very fact that MongoDB does
not bind the workload to CPUs and the
sharding the distribution of data between replicas sets in a cluster is
done by a background job now named the balancer this balancer is always running
always looking at how shouting should be done and always ensuring that data is
spread and balanced about the cluster it's not natural because it's not based
on consistent hashing it's something that it has to be calculated over and over again and then it splits the data
into chunks it's called in in in and then moves it around when this
happens this has a direct impact on the performance of your MongoDB cluster
because there is no isolation of this workload versus the rest of your real
production workload and that has significant and very bad impacts so the
TCO of a MongoDB cluster have separated both so the first one is on the cluster
itself and the second one is related to replicas sets the TCO is obviously bad
just to scale because you need it more right you had to create a monstrous
topology you waste resources because all those secondary nodes they are there
they have IO available but you are not using and that's that's bad in my opinion on
the up stretching this constant redistribution of data has a really
impact on how your class overall cluster performs and you can they offer some
time window where you can say I balanced do not run between peak hours let's say
but let's be honest it's just a patch on the replica set side though it should be
enough I mean the replica set itself is easy to reason with you might agree with
or not with the master/slave relationship but at the end of the day the TCO is pretty moderate and there's
no shame in scale in scaling up a master node or the nodes inside replica set
itself I think it's even saner than create a class a MongoDB cluster so go
for bigger machines scale up your iOS inside your me inside every one of the
nodes of your replica set that's okay and that relates well with what global
said just earlier about should I use community nodes or bigger nodes right the community promise in the no sequel
world I think doesn't stand anymore and this is a good example of it on the ops
rating I think it's it's really good actually there is almost no tuning needed for to
to operate and to set up a replica set and replica set automation it then
really easy because there are really few configuration options and the rolling a
braids are really smooth we never had a single problem of incompatibility when
upgrading our replica sets so it's it's fine on the silicide the consistent hashing
based clustering is really really efficient and it has tricks sure but
it's really efficient the TCO is then optimal in my opinion it's a clean
simple topology easy to return with you maximize your hardware virtualization
and you can scale horizontally virtually to infinity the up stretching could be
better I think and I know the Silla guys are putting a lot of efforts in making it better
so there are moderate number of configuration options if you look at the CLI ml which is a derivation of the
Cassandra ml you will you will see that there are a lot of options in it some of
them are not actually not used by Scylla which is a bit confusing some a bit
cryptic so you don't know so it's not really really easy to reason with
automation could be better as well there is still this panel manual setup required
maybe some things could be could be done a bit smoother and and of course they
are complex and mandatory background maintenance operations compactions which way is seamless and since Scylla
pinpoints workloads to CPUs they are able thanks to the scheduler to make
sure that these compactions do not have a big impact on your workload and repass
so unless you work at tv.com i really advise you to do them please okay
so there are semi seamless because they have put a lot of energy into Scylla manager and I'm eager to hear more about
what is coming on to bono let's look on the developers perspective now which is
Developers perspective
usually how database has a lot of traction and and becomes popular so at
the core we are comparing a document store with a colon Astro this document
store it supports geospatial queries text search aggregation pipelines graph
queries chain streams it's rich it's mature and it's really efficient so the
sunglasses really goes through to the document store and the kernel master has
UDT on Scylla and udg counters soon lightweight transactions and thank you
for this and all the other things also that like user-defined functions that
have you mentioned so we are eager to see them happening but for now
let's face it it's a bit more it split less it's limited they'll compare to the
document store so from a developer's perspective reasoning with a JSON object
is usually more flexible and feels more
natural than interacting with with a row
so this relates them to the modeling itself has a flexible schema you
can start inserting data into your collections which is the equivalent of a
table in in Silla with no effort and nothing will prevent you from doing this
since 3.6 you can also enforce and that's what I'm showcasing here a schema
so there is also the ability to have a schema validation before you insert data so that you make sure that your data
inside your collections are consistent which is pretty cool but at least you
got the choice versus the table table schemas which is fixed and even if in
the case on on a sequel world-altering is fine I mean it's it's almost seamless
this schema is there it's typed as well which if you don't apply a schema on
your documents you don't also don't have to type them so from a developer
perspective the entry barrier is lower on on MongoDB as well now let's talk
about connecting and interacting with the database which is a very very
important part in my opinion also in the adoption and in the in in the image that
database can have to the people that are actually using them and not operating
them here the kudos goes to PI so
this is my little minute where I showcase my love for this Python driver
which is I think a really an inspiration of how database drivers should
and and over the years the guys behind it mainly Jayceon and and and minute i
think it's its name have put a lot of energy and MongoDB as today a few years
ago all the crud operations they harmonized how all the all their drivers
interact with the database meaning that whether you're using Java Python or go
if you want to insert something into the database or fine and modify something in
the database the name of the function will be the same and the signature of the function will be the same it's the
only database I know that have undergone this full harmonization on how you
developers interact with the database so kudos to them it has been a and it came
from the guys behind pymongo so kudos to them for this it's it's really cool
Silla is for now relying and have been relying a lot on the Cassandra drivers
now we have some shallow well drivers as well that popping the Java was the first
one maybe or the good one I don't remember what it means that is usually
Cassandra and the ecosystem or clients of Cassandra and the drivers will interact well with Java based ecosystems
so such as a Dupin spark so on this side I think it's better to interact in
easier to interact with with with Hadoop on the syllabus spec chip than it is on
MongoDB because MongoDB has an abstraction reason which is a binary of the json representation that is not
native in this world so it it adds a bit of complexity to interact with with with
Hadoop and spark so here the Scylla Java driver which is shadow where and encourage you to use is
good the Cassandra connector when you interact with pack is good could be sure
it could benefit from improvements as well but there is still no shadow a
driver on Python which i think is a shame because data science has you know
is is a growing member of our communities that our communities so
their interaction with data base is crucial in their day to day work so the
most they can squeeze out of the database the better it is for them and
this data science world is mainly using Python so this is my second minute of
please guys make it happen I'm looking at door I think that's you
I don't but he must be somewhere like this around this please make it happen
it's something really I think important for our community let's see about
Querying
querying now all those aspects gives the
querying a frictionless querying on MongoDB it's really easy to issue
queries because you're just filtering edge in and interacting with JSON okay
so and we have seen that there are a lot of things that you can do already whereas in Silla I don't know why I'd
say whereas because it's also pretty easy to reason with on the Silla world
there are performance warranties queries which means that from the developer's
perspective it has something a bit weird on each of them when you issue queries
on MongoDB you can issue any kind of query the database will just accept it
so it's cool striction less and you're happy at the beginning and least but you
don't see that you are actually running a cold scan what is called a color scan so it's like a full table scan on a
simple looking query so you can end up affecting your production workload by
issuing wrong queries that are sub suboptimal so that's that's not good and that's mostly
hidden from you by this nice and shiny flexibility of Silla won't let you
do that at all it will just say eh you are asking me to do something that will
have unpredictable performance please think again or add these low filtering
magic words and I'll do it anyway so it formed a developer perspective and I
know about it because I've been advocating a lot at memory with my
developer teams they were like oh I would like to issue this button I'm getting thrown away I said it's fine
it's for your own good it was also for
the database good and that anyway at the end of the day it's true so think about
your modeling thing about something else but try to avoid them unless you know of course what you're doing we do actually
use a lot of low filtering queries handling consistency the key word here
is to never buy the clients so that's up to the clients to have their own
consistency depending on what it is that they are doing with the with the cluster it's just the principle is this about
the same it's how many nodes of my topology should acknowledge the operation that I'm asking for right now
it's called right concern on MongoDB and it's called consistency level on Scylla
but you can generalize where that it's basically the same idea and that it's
the clients responsibility to actually choose about this again unless you work
at key we do not forget repairs please repairs and a lot of people can explain
this better than me they can if you forget them and you delete data they can lead to resurrected data and I know even
if it's Halloween season you don't want some B data in your cluster please so do not work
repairs data exploration now you can
expire data on both systems on MongoDB it's done by adding a special field that
is meant just for this that we will create a special index with an expire
after second option that will tell MongoDB hey this data can be deleted at
this time so the cost of handling TTL in is actually an index and a special
field which is okay most of the time but it still has a cost
remember it on Cilla side all of this is already included
because the may it's it's part of the metadata that is collected by column so
you can have a general TTL on the table definition or you can set it on insert
update it's pretty easy to reason live so you can expire a colon or you can
expire a row where as will just expire the whole document it's
interesting to note that you can query the TTL there is the TTL function that will allows you to query the expiration
of certain Conan it doesn't work on primary anchor and and core and
clustering keys another thing interesting is that this
in this metadata you will also get free write timestamps so a lot of people
usually have a special feel for this like created at write that we that we
keep on our our data models and every you can see them about everywhere it's
of course a really valid use case but just in case you didn't know you have this right time function in insula that
will give you this for free because it's part of the metadata so if we some a
Performance
puppet on the developer perspective what we can see and fairly say is that
MongoDB flavors favors flexibility to performance it's easy to
interact with it will not get on your way basically so it's pretty cool but it
will have impact on it will have impacts on performance of course it's still no
sequel world so impacts on performance if it's millisecond and it's okay for you it's fine but it's not always the
case for for every workload Sylla on the other hand the favors consistent performance - versatility so
it looks a bit more fixed and bit more rigid on the outside but once again
that's for your own good and so you can have consistent performance and operate well and interact well with with the
with the system this makes in my opinion
Conclusion
a real difference when you have a lot of work load that is latency and
performance sensitive like we do the list is not full okay but it's good that
we now have efficiency side filtering those these since 3.0 this low filtering
got a lot improved and and its footprint on the usage of the cluster has
dramatically reduced so it's good news row level repairs that just got
announced we are going to have in three point two and simple but yet efficient
like operator so you can do finally do like something and use special combos
and globs inside inside queries to look into text field so that's something that
actually we have been need asking the Scylla guys to implement and and pretty
quickly did it so thank you again for for taking our requests into account and
making it happen is going to help us a lot because for now we are doing it on the application side and it's leveraging
on the efficient server-side filtering that was implementing triple o light when transactions of
if you're Cassandra user you have been expecting it for a long time I was
really happy to to see that we will get two flavors of it I was expecting the
rough one but not the paxos one so it's really cool change data capture pour everyone who interacts with Scott which
cap cap for instance like we do it's really really important and that's something that we have been waiting for
a while to be honest UDF's and and so on
I guess we'll we have a lot of nice news and that will be provided along the
conference still missing in disinter sections so this is something that I
know they are going to work on because today you can have multiple index but
the way they are chosen by the engine insula is not always optimal and things
could be better there once again sorry but a good and shadow a Python driver
please please please and it's asynchronous to using a Sicario and
search capabilities so last year they promised it was something that they had in mind so I'm taking their word for it
and I'm looking forward to hear more about search capabilities of Scylla
because for some easy things it might be handy not to have to have a separate
elastica thing to maintain and another nightmare in the end that's actually and
great no sequel combo so it's not deathmatch after all we are happy users
of both you just have and we do are happy because we select the use cases
based on their strengths let's lay let's get into some of those use cases at
number Li MongoDB is very strong in web backends and real-time queries over
unpredictable behavioral data so we get fitted with data that comes from let's
say web tracking data and that is implemented from our clients perspective
so in their website so every time someone this is their website we do Cole I will have it it's cool but we
don't have a strict manner to impose a schema on this and we still need to be
able to query this data to look at what it looks like and to do some operations
on it and because we are real-time pipelines taking actions based on what they see this is where is and
flexibility is good for us because it allows us to just store it somewhere and query it fine on the Scylla side we use
it for real-time and of course latency sensitive data pipelines so that is was
likely data enrichment where you have multiple sources of data and you want to correlate them in real time on pipeline
so that's tricky to do and latency sensitive we also mix a lot of batch and
real-time workloads in Scylla because it gives us the best of both worlds so
that's the torque I gave last year and that was explaining how we had - one pot
and MongoDB on the other but we put everything in Scylla and they could be sustaining Hadoop like batch workloads
and real-time pipelines work good so that's where Scylla is shining at number
D and also more and more of our web backends are done in graph QL which
imposes as well as schema and in this case of having a schema based API having
a schema based database with low latency and high availability is really good so
a lot of our back-end engineer's and front end as well since now gradually
fills the gap are really cool adopters of Scylla we see the trend to be honest
really into - into Scylla today so we have more and more doctors at number
early and more more tech people coming and asking a I have this use case would
it fit and would it be fun to start using it on Scylla and most of the time
the answer is yes so oh the silly adoption is still growing MongoDB is flat but is still here to
stay because it has some really interesting features as long as you don't do a MongoDB cluster please

# MongoDB versus Cassandra

| Data Modelling | general purpose document store | partitioned row store<br>(store a column family in a row-by-row fashion)<br>(columns are part of the data and not part of the schema |
| --- | --- | --- |
|  | BSON | Log-structured merge trees(LSM) |
|  | <br>databases -> collections -> documents -> columns -> values | Keyspace -> column families  -> rows -> ordered columns -> (column name + column value + timestamp) |
|  | flexibility around no of columns in any document | flexibility around no of columns in any document |
|  | Each collection visualised as List<JSON> | each column family represented as nested sorted map<br>Map<RowKey, SortedMap<ColumnKey, ColumnValue>> |
|  |  |  |
| IO Operation | complete document gets picked | group of columns in a column family gets picked |
|  |  |  |
| Query patterns | query patterns can evolve as indexes can be added later for different columns | need upfront clarity around columns to be access together and data modelling should be done accordingly |
|  | write flow<br>read from disk -> modify->update to disk | write flow - suitable for write heavy workloads<br>write in memtable -> flush at regular interval to SSTable/disk(seq IO - append mode) |
|  |  |  |
| Choose criteria | data is more dynamic, <br>query patterns can evolve<br>CP(write flow availability sacrificed when leader is down or disconcected due to n/w partition) | 1. Writes exceed reads by a large margin. <br>2. Immutable append only data with almost no updates/deletes.<br>3. reads are very targeted(by primary key)<br>4. Data is rarely updated and when updates are made they are idempotent.<br>5. Data can be partitioned for linear scaling.<br>6. There is no need for joins or aggregates.<br>7. No ACID or locks use cases<br><br>Realworld use cases<br>1. tracking: sensors data, health tracker, weather, iot of cars trucks, ecommerce order/shipment status changes. |
|  |  |  |
| example - use cases | blogging website | sensors data, health tracker, weather, iot of cars trucks, ecommerce order/shipment status changes. |

<br />
<br />

# CAP Theorem
1. impossible for a distributed data store to simultaneously provide more than two of the three guarantees.
![](https://github.com/khatwaniNikhil/choosing_right_database/blob/main/CAP_theorem.png)

<br />
<br />

# Tradeoffs based decision
1. ACID support | OLTP workloads | tunable for CA,CP,AP based on needs(https://stackoverflow.com/questions/29663645/why-are-rdbms-considered-available-ca-for-cap-theorem) - **RDBMS**    
3. Focus on Availability and Partition Tolerance (AP) | fast and cheap scalability is mandatory | Big Data category | Eventual Consistency is tolerable - **NoSQL**
4. ACID | OLTP workloads | horizontal Scalability - **New SQL**
   1. Examples:
       1. Citus extension for PostgresSQL
       2. YugabyteDB
       3. Vitess
5. ACID | JOINS | Flexible schema | Complex query patterns - PostgreSQL(leverage table inheritance) + ZomboDB plugin based Elasticsearch index

<br />
<br />

# NOSQL
## KEY VALUE STORES (Redis, Riak, Memcache, Amazon Dynamodb(supports json based document store data model)
1. not designed to be **field-queried**:
   1. value part is opaque to client/consumer of data - query by any field within value/json will lead to full scan across all partitons
2. recommended for **Session storage | Store user preferences |  Shopping cart**
   1. supporting  TTL for records
3. with right partition and record key, crud is blazing fast
4. no ACID based txn support
5. can’t access muliple key value pairs in single db request
6. visualise data
   1. spread across partitions(with a partition key based lookup) — Partition(UserID)
   2. within each partition, each record has record key/id and value(json —text, media etc.) —— UserID.Name, UserID.Location, UserID.Height
   3. If a physical partition contains several hot logical partitions (e.g. a set of users that request data frequently), engine is able to distribute the logical partitions spreading the workload across different clusters. 

## Document oriented database(MongoDB, Amazon DocumentDB)
1. recommended for **catalogs | user profiles | and content management systems**
2. supports JSON-like documents - flexible, semistructured, and hierarchical nature
3. Documents map to objects in application layer 
4. can be adhoc field queried, flexible indexing - db does indexing of fields of the document/secondary indexes
5. extends the key value model and values are structured format(called as document- JSON, BSON, or XML)
6. joins are supported(different from rdbms joins)

## Column oriented DB(Cassandra, HBase)
1. Data Warehousing |  analytical applications
2. Column-oriented storage improves analytic query performance because it drastically reduces the overall disk I/O requirements
3. compression scheme selected specifically for the column data type can make storage more optimised

## Graph Databases
