	• Cloudera impala is the massively parallel execution(MPP) engine that runs on the hadoop platform.
	• It can be used to run a query ,evaluate results immediately and fine tune the query if necessary
	• Analyze hadoop data via sql or other business intelligence tools
	• Perform interactive, adhoc and batch queries together in hadoop system.
	
Benefits of impala:
	
	• Flexibility: integrates well with existing hadoop components
	• Enhanced sql query speed : enables interactive query
	• Enables user to query low latency sql data from hdfs and apache hbase without causing any data movement or transformation.

Data storage
	• It uses two media for data storage 1.HDFS 2.HBASE
Managing metadata
	• Impala uses same metastructure as hive
	• It maintains table definition information in a central database called the metastore (mysql/posgresql)
	• Each impala node caches all metadata to reuse in future queries against same table.

	
Difference between hive and impala

Hive is batch based mapreduce where as Impala (Created by cloudera distribution) is more like MPP database (Example. vertica).
If you run a query on hive there is starttime overhead on queries run on mapreduce but not on impala.
Fault tolerance
Hive is fault tolerant where as impala is not. For e.g. if you run a query in hive mapreduce and while the query is running one of your datanode goes down still the output would be produced as its fault tolerant. It's not the same with Impala and if the query fails you will have to start the query all over again.
Memory based
Impala is more memory bound which makes it multitude faster than disk based mapreduce.  All heavy calculations like group by, conversions would be memory based.
Note: - in newest versions of impala there are settings which can make it spill to the disk if the memory overflows.
Impala while reading data is multinode but while writing data it uses single virtual disk. I guess when we have recently met cloudera representatives they discussed impala 2.5 (CDH 5.7 version) which might release in may (not sure though) and will have capability of writing data multinode.

Resource Pooling
Hive i.e. mapreduce is supported by YARN. So you can manage your resources for mapreduce or any other applications supported by YARN. Impala, is not currently supported by YARN (Note: - you can use llama but its not currently supported).
We run impala with static memory allocation (recommended atleast 128 GB per node).
These are the major difference between Hive and Impala. For looking at syntactical differences while coding queries you will have to actually write queries :) and see.
