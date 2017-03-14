distributed database (no single point of failure)
gossip communication protocol
Automatic data distribution
replication (to nodes in ring)
linear scalability
Nosql( uses a method of storage different from a relational)
CQL (difference between sql and cqlis ,cql doesnt support joins or subqueries)
CQL shell (cqlsh)
with CQL we can create keyspaces ,tables and insert data into tables
commit log (to ensure data durability)
memtable ( Data is then indexed and written to an in-memory structure)
SSTables 

#####partitioning in cassandra
A partitioner determines how data is distributed across the nodes in the cluster.

In Cassandra, the total amount of data managed by the cluster is represented as a ring. The ring is divided into ranges
equal to the number of nodes, with each node being responsible for one or more ranges of the data. Before a node can
join the ring, it must be assigned a token.


The main class of Storage Layer is StorageProxy. It handles all the requests.

Messaging Layer is responsible for internode communications like gossip. Apart
from this, process-level structures keep a rough idea about the actual data containers
and where they live. 

There are four data buckets that you need to know. 

#####MemTable
is a hash table-like structure that stays in memory. It contains actual column data.
SSTable is the disk version of MemTables. When MemTables are full, SSTables are
persisted to the hard disk. 

*Bloom filters* is a probabilistic data structure that lives
in memory. It helps Cassandra to quickly detect which SSTable does not have the
requested data.

CommitLog is the usual commit log that contains all the mutations
that are to be applied. It lives on the disk and helps to replay uncommitted changes.

#####coordinator node
to write/read  data into/from cassandra ,client needs to connect to any one of the cassandra node and send a write request.

#####storage proxy
it will be responisble for identifying repicated nodes to hold the data. It send row mutation message to the nodes and waits for reply from them.

#####Failure detector
If FailureDetector detects that there aren't enough live nodes to satisfy
ConsistencyLevel, the request fails.

If FailureDetector gives a green signal, but writes time-out after the request
is sent due to infrastructure problems or due to extreme load, StorageProxy
writes a local hint to replay when the failed nodes come back to life. This is
called hinted handoff.

If the replica nodes are distributed across datacenters, it will be a bad idea
to send individual messages to all the replicas in other datacenters. It rather
sends the message to one replica in each datacenter with a header instructing
it to forward the request to other replica nodes in that datacenter.

Now, the data is received by the node that should actually store that data.
The data first gets appended to CommitLog, and pushed to a MemTable for
the appropriate column family in the memory.

#####compaction process
When MemTable gets full, it gets flushed to the disk in a sorted structure
named SSTable. With lots of flushes, the disk gets plenty of SSTables. To
manage SSTables, a compaction process runs. This process merges data from
smaller SSTables to one big sorted file.


Similar to a write case, when StorageProxy of the node that a client is connected
to gets the request, it gets a list of nodes containing this key based on Replication
Strategy. StorageProxy then sorts the nodes based on their proximity to itself.
The proximity is determined by the Snitch function that is set up for this cluster.

#####SimpleSnitch
A closer node is the one that comes first when moving
clockwise in the ring.

#####AbstractNetworkTopologySnitch

#####DynamicSnitch

Each SSTable is associated with its Bloom Filter built on the row keys in the SSTable.
Bloom Filters are kept in memory, and used to detect if an SSTable may contain (false
positive) the row data. Now, we have the SSTables that may contain the row key.
The SSTables get sorted in reverse chronological order (latest first).



Apart from Bloom Filter for row keys, there exists one Bloom Filter for each row in
the SSTable. This secondary Bloom Filter is created to detect whether the requested
column names exist in the SSTable. Now, Cassandra will take SSTables one by one
from younger to older. And use the index file to locate the offset for each column
value for that row key and the Bloom filter associated with the row (built on the
column name). On Bloom filter being positive for the requested column, it looks into
the SSTable file to read the column value. Note that we may have a column value
in other yet-to-be-read SSTables, but that does not matter, because we are reading
the most recent SSTables first, and any value that was written earlier to it does not
matter. So, the value gets returned as soon as the first column in the most recent
SSTable is allocated.

distributed and decentralised
Cassandra is distributed and decentralized, there is no single point
of failure(SPOF), which supports high availability.(doenot follow master slave architecture)

distibuted : capable of running on multiple
machines while appearing to users as a unified whole.
peer-to-peer protocol and uses gossip to maintain and
keep in sync a list of nodes that are alive or dead.

Decentralization, therefore, has two key advantages: it’s simpler to use than master/
slave, and it helps you avoid outages. It can be easier to operate and maintain a decentralized
store than a master/slave store because all nodes are the same. That means that
you don’t need any special knowledge to scale; setting up 50 nodes isn’t much different
from setting up one.

Elastic scalability
Elastic scalability refers to a special property of horizontal scalability. It means that
your cluster can seamlessly scale up and scale back down. To do this, the cluster must
be able to accept new nodes that can begin participating by getting a copy of some or
all of the data and start serving new user requests without major disruption or reconfiguration
of the entire cluster. You don’t have to restart your process. You don’t have
to change your application queries. You don’t have to manually rebalance the data
yourself. Just add another machine—Cassandra will find it and start sending it work.
Scaling down, of course, means removing some of the processing capacity from your
cluster.

High Availability and Fault Tolerance
Cassandra is highly available. You can replace failed nodes in the cluster with no downtime, and you can replicate data to multiple data centers to offer improved local performance and prevent downtime if one data center experiences a catastrophe such as fire or flood.

Tunable consistency (latency) tracing consistency (one-)
Consistency essentially means that a read always returns the most recently written
value.
