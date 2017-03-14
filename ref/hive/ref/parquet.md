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


