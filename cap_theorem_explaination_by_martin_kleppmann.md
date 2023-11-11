# CAP Theorem
https://martin.kleppmann.com/2015/05/11/please-stop-calling-databases-cp-or-ap.html

## CAP -Consistency - Linearizability
1. UserB making req after successfull userA completed request should see same state as updated/shown to user A
2. fairly expensive guarantee to provide, because it requires a lot of coordination
   Even the CPU in your computer doesn’t provide linearizable access to your local RAM! On modern CPUs, 
   you need to use an explicit memory barrier instruction in order to get linearizability.
3. And even testing whether a system provides linearizability is tricky.

## ACID/RDBMS consistency
1. Data updates always honor rules like PK,FK,unique, not null constraints, triggers etc.
2. Example: money transfer balance cannot be negative


## CAP-Availability
1. Formal definition/NotPractical in real world
	every request received by a non-failing node in the system must result in a [non-error] response
	CAP theorem says nothing about latency

2. Practically availability is defined as non error response within a given percentile based SLA

## CAP - Parition tolerance
1. since Network is unreliable so inter communication between will be affected and cannot be avoided.

## CA versus CP example(applies within a single dc also)
1. Applcn with database with cross dc replication(could be single leader(master/slave), multi leader(master-master), quorom based
2. Requirement - on user change of data, it should replicate across diff data centers.
   Assume datalink across datacenters and customer can connect to one dc at a time
3. Network link across dc's is interrupted leading to n/w partion among nodes.
4. Choices
	1. CA - For high availability, allow writes in both dc's but due to partition user might not get latest data/Linearizability is not guaranteed.
	2. CP - For high consistency, one of the dc stop accepting read and writes and other becomes leader until network parition is not resolved.
	   Data center which stop accepting user requests has not failed but has stopped taking user requests.
	   availability from non failing node perspective is affected. It does not necessarily means outage within application if we can
	   route all the traffic from one dc to another dc here.


## Mysql/Postgres/Oracle
single leader with async replication is 
1. Not CAP-available  - when leader is down, writes are blocked
2. Not CAP-consistent - async replication with delays, linearizabiity is not guaranteed

## MongoDB
1. Not CAP-available  - single leader per shard and when that leader is down, writes are blocked on that shard
2. Not CAP-consistent - non-linearizable reads even at the highest consistency setting
https://aphyr.com/posts/322-call-me-maybe-mongodb-stale-reads


1. impossible for a distributed data store to simultaneously provide more than two of the three guarantees.
![](https://github.com/khatwaniNikhil/choosing_right_database/blob/main/CAP_theorem.png)

<br />
<br />

