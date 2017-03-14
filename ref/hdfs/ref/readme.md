	• HDFS is a distributed file system on top of that we have all other eco systems like hive ,pig ,hbase, oozie etc
	• Daemons are called background process.

Master:
Name node(metadata)
Job tracker
Secondary namenode

Slaves:
Task tracker
Data node

heartbeats
--->client API(application program interface java code) is responsible for request to namenode and getting response from name node.

pivotal 3.4

hortonswork

core-site.xml

Temp dir, configuration, property,name,value(location),final

hdfs-site.xml

replication factor, block size

dfs.name.dir ,dfs.data.dir

details related to data node,name node

mapred-site.xml

details related to job tracker ,task tracker
