Bucketing  decreases the data into more manageable parts or equal parts. The value of the bucketing  column will be hashed by a defined number into buckets. Records with the same column will always be stored in the same bucket.
* We can perform Hive bucketing optimization only on one column only not more than one.

#####What exact differences between partitioning and bucketing is that

Partitioning will create directories for column values whereas Bucketing divides the specified column into files.
* We can perform partition on any number of columns in a table by using hive partition concept.
* We can perform Hive Partitioning concept on Hive Tables like Managed tables or External tables
* Partitioning is works better when the cardinality of the partitioning field is not too high .
Partitioning  helps in organizing data in a logical fashion
* Due to equal volumes of data in each partition, joins at Map side will be quicker.
In dynamic Partitioning ,the directories will grow. While in bucketing the files will be fixed as we will mention no of buckets at the time of table creation

#####Disadvantages 
 For partitioning: possibility for creating too many folders in HDFS that is extra burden for Namenode metadata.

So, bucketing works well when the field has high cardinality and data is evenly distributed among buckets. Partitioning works best when the cardinality of the partitioning field is not too high.
