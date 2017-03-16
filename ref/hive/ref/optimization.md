- [Partitioning](#Partitioning)
- [Bucketing](#Bucketing)
- [Vectorization](#Vectorization)
- [File formats](#File formats)
- [Compression Techniques](#Compression Techniques)

## Partitioning

Partitioning which is physical division of data which we have to specify while table creation. While selecting partition column cardinality should be low( column with less values).And again while coming to partition we have two types 
 1.  Static partitioning  :  is the partitioning in which we will hardcode the column values.
 2. Dynamic partitioning , the partitions will be created dynamically based on column values so there is no hard coded column values

To enable dynamic partition inserts
**set hive.exec.dynamic.partition=true;**

In strict mode, the user must specify at least one static partition in case the user accidentally overwrites all partitions, in nonstrict mode all partitions are allowed to be dynamic.
**set hive.exec.dynamic.partition.mode=nonstrict**

> CREATE TABLE pageviews (userid VARCHAR(64), link STRING, came_from STRING)
>  PARTITIONED BY (datestamp STRING) CLUSTERED BY (userid) INTO 256 BUCKETS STORED AS ORC

## Bucketing

Bucketing  decreases the data into more manageable parts or equal parts. The value of the bucketing  column will be hashed by a defined number into buckets. Records with the same column will always be stored in the same bucket.
* We can perform Hive bucketing optimization only on one column only not more than one.

##### What exact differences between partitioning and bucketing is that

Partitioning will create directories for column values whereas Bucketing divides the specified column into files.
* We can perform partition on any number of columns in a table by using hive partition concept.
* We can perform Hive Partitioning concept on Hive Tables like Managed tables or External tables
* Partitioning is works better when the cardinality of the partitioning field is not too high .
Partitioning  helps in organizing data in a logical fashion
* Due to equal volumes of data in each partition, joins at Map side will be quicker.
In dynamic Partitioning ,the directories will grow. While in bucketing the files will be fixed as we will mention no of buckets at the time of table creation

##### Disadvantages
 For partitioning: possibility for creating too many folders in HDFS that is extra burden for Namenode metadata.

So, bucketing works well when the field has high cardinality and data is evenly distributed among buckets. Partitioning works best when the cardinality of the partitioning field is not too high.

## Vectorization

## File formats

## Compression techniques



