## Optimization in hive 

- [Partitioning](#partitioning)
- [Bucketing](#bucketing)
- [Vectorization](#vectorization)
- [File formats](#file formats)
- [Compression Techniques](#compression Techniques)

## Partitioning

Partitioning which is physical division of data which we have to specify while table creation. While selecting partition column cardinality should be low( column with less values).And again while coming to partition we have two types 
 1.  Static partitioning  :  is the partitioning in which we will hardcode the column values.
 2. Dynamic partitioning : the partitions will be created dynamically based on column values so there is no hard coded column values

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

### Avro

Avro files are quickly becoming the best multi-purpose storage format within Hadoop. Avro files store metadata with the data but also allow specification of an independent schema for reading the file. This makes Avro the epitome of schema evolution support since you can rename, add, delete and change the data types of fields by defining new independent schema.

 Additionally, Avro files are splittable, support block compression and enjoy broad, relatively mature, tool support within the Hadoop ecosystem.

### JSON

JSON records are different from JSON Files in that each line is its own JSON datum -- making the files splittable. Unlike CSV files, JSON stores metadata with the data, fully enabling schema evolution. However, like CSV files, JSON files do not support block compression. Additionally, JSON support was a relative late comer to the Hadoop toolset and many of the native serdes contain significant bugs. Fortunately, third party serdes are frequently available and often solve these challenges. You may have to do a little experimentation and research for your use cases. 

### ORC

ORC Files or Optimized RC Files were invented to optimize performance in Hive and are primarily backed by HortonWorks. ORC files enjoy the same benefits and limitations as RC files just done better for Hadoop. This means ORC files compress better than RC files, enabling faster queries. However, they still don’t support schema evolution. Some benchmarks indicate that ORC files compress to be the smallest of all file formats in Hadoop. It is worthwhile to note that, at the time of this writing, Cloudera Impala does not support ORC files.

### RC

RC Files or Record Columnar Files were the first columnar file format adopted in Hadoop. Like columnar databases, the RC file enjoys significant compression and query performance benefits. However, the current serdes for RC files in Hive and other tools do not support schema evolution. In order to add a column to your data you must rewrite every pre-existing RC file. Also, although RC files are good for query, writing an RC file requires more memory and computation than non-columnar file formats. They are generally slower to write. 

### PARQUET

Parquet Files are yet another columnar file format that originated from Hadoop creator Doug Cutting’s Trevni project. Like RC and ORC, Parquet enjoys compression and query performance benefits, and is generally slower to write than non-columnar file formats. However, unlike RC and ORC files Parquet serdes support limited schema evolution. In Parquet, new columns can be added at the end of the structure. At present, Hive and Impala are able to query newly added columns, but other tools in the ecosystem such as Hadoop Pig may face challenges. Parquet is supported by Cloudera and optimized for Cloudera Impala. Native Parquet support is rapidly being added for the rest of the Hadoop ecosystem.
One note on Parquet file support with Hive... It is very important that Parquet column names are lowercase. If your Parquet file contains mixed case column names, Hive will not be able to read the column and will return queries on the column with null values and not log any errors. Unlike Hive, Impala handles mixed case column names. A truly perplexing problem when you encounter it!

Apache Parquet is a columnar storage format available to any component in the Hadoop ecosystem, regardless of the data processing framework, data model, or programming language. The Parquet file format incorporates several features that support data warehouse-style operations:

* Columnar storage layout - A query can examine and perform calculations on all values for a column while reading only a small fraction of the data from a data file or table.
* Flexible compression options - Data can be compressed with any of several codecs. Different data files can be compressed differently.
* Innovative encoding schemes - Sequences of identical, similar, or related data values can be represented in ways that save disk space and memory. The encoding schemes provide an extra level of space savings beyond overall compression for each data file.
* Large file size - The layout of Parquet data files is optimized for queries that process large volumes of data, with individual files in the multimegabyte or even gigabyte range.

## Parquet File Structure
To examine the internal structure and data of Parquet files, you can use the parquet-tools command that comes with CDH. Make sure this command is in your $PATH. (Typically, it is symlinked from /usr/bin; sometimes, depending on your installation setup, you might need to locate it under a CDH-specific bin directory.) The arguments to this command let you perform operations such as:
cat: Print a file's contents to standard out. In CDH 5.5 and higher, you can use the -j option to output JSON.
head: Print the first few records of a file to standard output.
schema: Print the Parquet schema for the file.
meta: Print the file footer metadata, including key-value properties (like Avro schema), compression ratios, encodings, compression used, and row group information.
dump: Print all data and metadata.
Use parquet-tools -h to see usage information for all the arguments. Here are some examples showing parquet-tools usage:

##Using Parquet in Hive
#### Hive 0.10 - 0.12

CREATE TABLE parquet_test (
 id int,
 str string,
 mp MAP<STRING,STRING>,
 lst ARRAY<STRING>,
 strct STRUCT<A:STRING,B:STRING>) 
PARTITIONED BY (part string)
ROW FORMAT SERDE 'parquet.hive.serde.ParquetHiveSerDe'
 STORED AS
 INPUTFORMAT 'parquet.hive.DeprecatedParquetInputFormat'
 OUTPUTFORMAT 'parquet.hive.DeprecatedParquetOutputFormat';

#### Hive 0.13 and later

set parquet.compression=GZIP;

CREATE TABLE parquet_test (
 id int,
 str string,
 mp MAP<STRING,STRING>,
 lst ARRAY<STRING>,
 strct STRUCT<A:STRING,B:STRING>) 
PARTITIONED BY (part string)
STORED AS PARQUET;

#Using Parquet in Pig
### Reading Parquet Files in Pig
If the external table is created and populated, the Pig instruction to read the data is:

grunt> A = LOAD '/test-warehouse/tinytable' USING parquet.pig.ParquetLoader AS (x: int, y  int);
### Writing Parquet Files in Pig
Create and populate a Parquet file with the ParquetStorer class:

grunt> store A into '/test-warehouse/tinytable' USING parquet.pig.ParquetStorer;

#####To set the compression type, configure the parquet.compression property before the first store instruction in a Pig script:

SET parquet.compression gzip;

#Using parquet in Spark


### TEXT

CSV files are still quite common and often used for exchanging data between Hadoop and external systems. They are readable and ubiquitously parsable. They come in handy when doing a dump from a database or bulk loading data from Hadoop into an analytic database. However, CSV files do not support block compression, thus compressing a CSV file in Hadoop often comes at a significant read performance cost. When working with Text/CSV files in Hadoop, never include header or footer lines. Each line of the file should contain a record. This, of course, means that there is no metadata stored with the CSV file. You must know how the file was written in order to make use of it. Also, since the file structure is dependent on field order, new fields can only be appended at the end of records while existing fields can never be deleted. As such, CSV files have limited support for schema evolution.


### SEQUENCE

Sequence files store data in a binary format with a similar structure to CSV. Like CSV, sequence files do not store metadata with the data so the only schema evolution option is appending new fields. However, unlike CSV, sequence files do support block compression. Due to the complexity of reading sequence files, they are often only used for “in flight” data such as intermediate data storage used within a sequence of MapReduce jobs. 

## Compression techniques



