Partitioning which is physical division of data which we have to specify while table creation. While selecting partition column cardinality should be low( column with less values).And again while coming to partition we have two types 
 1.  Static partitioning  :  is the partitioning in which we will hardcode the column values.
 2. Dynamic partitioning , the partitions will be created dynamically based on column values so there is no hard coded column values

To enable dynamic partition inserts
**set hive.exec.dynamic.partition=true;**

In strict mode, the user must specify at least one static partition in case the user accidentally overwrites all partitions, in nonstrict mode all partitions are allowed to be dynamic.
**set hive.exec.dynamic.partition.mode=nonstrict**

> CREATE TABLE pageviews (userid VARCHAR(64), link STRING, came_from STRING)
>  PARTITIONED BY (datestamp STRING) CLUSTERED BY (userid) INTO 256 BUCKETS STORED AS ORC
