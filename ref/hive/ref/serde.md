## Built in serde

Hive has 3 main components:
Serializers/Deserializers (trunk/serde) - This component has the framework libraries that allow users to develop serializers and deserializers for their own data formats. This component also contains some builtin serialization/deserialization families.
MetaStore (trunk/metastore) - This component implements the metadata server, which is used to hold all the information about the tables and partitions that are in the warehouse.
Query Processor (trunk/ql) - This component implements the processing framework for converting SQL to a graph of map/reduce jobs and the execution time framework to run those jobs in the order of dependencies.

SerDe is short for Serializer/Deserializer. Hive uses the SerDe interface for IO. The interface handles both serialization and deserialization and also interpreting the results of serialization as individual fields for processing.
A SerDe allows Hive to read in data from a table, and write it back out to HDFS in any custom format. Anyone can write their own SerDe for their own data formats.
The Hive SerDe library is in org.apache.hadoop.hive.serde2.

### Input Processing
Hive's execution engine (referred to as just engine henceforth) first uses the configured InputFormat to read in a record of data (the value object returned by the RecordReader of the InputFormat).
The engine then invokes Serde.deserialize() to perform deserialization of the record. There is no real binding that the deserialized object returned by this method indeed be a fully deserialized one. For instance, in Hive there is a LazyStruct object which is used by the LazySimpleSerDe to represent the deserialized object. This object does not have the bytes deserialized up front but does at the point of access of a field.
The engine also gets hold of the ObjectInspector to use by invoking Serde.getObjectInspector(). This has to be a subclass of structObjectInspector since a record representing a row of input data is essentially a struct type.
The engine passes the deserialized object and the object inspector to all operators for their use in order to get the needed data from the record. The object inspector knows how to construct individual fields out of a deserialized record. For example, StructObjectInspector has a method called getStructFieldData() which returns a certain field in the record. This is the mechanism to access individual fields. For instance ExprNodeColumnEvaluator class which can extract a column from the input row uses this mechanism to get the real column object from the serialized row object. This real column object in turn can be a complex type (like a struct). To access sub fields in such complex typed objects, an operator would use the object inspector associated with that field (The top level StructObjectInspector for the row maintains a list of field level object inspectors which can be used to interpret individual fields).
For UDFs the new GenericUDF abstract class provides the ObjectInspector associated with the UDF arguments in the initialize() method. So the engine first initializes the UDF by calling this method. The UDF can then use these ObjectInspectors to interpret complex arguments (for simple arguments, the object handed to the udf is already the right primitive object like LongWritable/IntWritable etc).
### Output Processing
Output is analogous to input. The engine passes the deserialized Object representing a record and the corresponding ObjectInspector to Serde.serialize(). In this context serialization means converting the record object to an object of the type expected by the OutputFormat which will be used to perform the write. To perform this conversion, the serialize() method can make use of the passed ObjectInspector to get the individual fields in the record in order to convert the record to the appropriate type.

- [Avro](#avro)
- [ORC](#orc)
- [Regex](#regex)
- [Thrift](#thrift)
- [Parquet](#parquet)
- [CSV](#csv)
- [Jsonserde](#jsonserde)


# Avro

To create an Avro-backed table, specify the serde as org.apache.hadoop.hive.serde2.avro.AvroSerDe, specify the inputformat as org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat, and the outputformat as org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat. Also provide a location from which the AvroSerde will pull the most current schema for the table. For example:
CREATE TABLE kst
  PARTITIONED BY (ds string)
  ROW FORMAT SERDE
  'org.apache.hadoop.hive.serde2.avro.AvroSerDe'
  STORED AS INPUTFORMAT
  'org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat'
  OUTPUTFORMAT
  'org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat'
  TBLPROPERTIES (
    'avro.schema.url'='http://schema_provider/kst.avsc');
    
 In this example we're pulling the source-of-truth reader schema from a webserver. Other options for providing the schema are described below.
Add the Avro files to the database (or create an external table) using standard Hive operations (http://wiki.apache.org/hadoop/Hive/LanguageManual/DML).
This table might result in a description as below:
hive> describe kst;
OK
string1 string  from deserializer
string2 string  from deserializer
int1    int     from deserializer
boolean1        boolean from deserializer
long1   bigint  from deserializer
float1  float   from deserializer
double1 double  from deserializer
inner_record1   struct<int_in_inner_record1:int,string_in_inner_record1:string> from deserializer
enum1   string  from deserializer
array1  array<string>   from deserializer
map1    map<string,string>      from deserializer
union1  uniontype<float,boolean,string> from deserializer
fixed1  binary  from deserializer
null1   void    from deserializer
unionnullint    int     from deserializer
bytes1  binary  from deserializer

Hive 0.14 and later versions
Starting in Hive 0.14, Avro-backed tables can simply be created by using "STORED AS AVRO" in a DDL statement. AvroSerDe takes care of creating the appropriate Avro schema from the Hive table schema, a big win in terms of Avro usability in Hive.
For example:
CREATE TABLE kst (
    string1 string,
    string2 string,
    int1 int,
    boolean1 boolean,
    long1 bigint,
    float1 float,
    double1 double,
    inner_record1 struct<int_in_inner_record1:int,string_in_inner_record1:string>,
    enum1 string,
    array1 array<string>,
    map1 map<string,string>,
    union1 uniontype<float,boolean,string>,
    fixed1 binary,
    null1 void,
    unionnullint int,
    bytes1 binary)
  PARTITIONED BY (ds string)
  STORED AS AVRO;
This table might result in a description as below:
hive> describe kst;
OK
string1 string  from deserializer
string2 string  from deserializer
int1    int     from deserializer
boolean1        boolean from deserializer
long1   bigint  from deserializer
float1  float   from deserializer
double1 double  from deserializer
inner_record1   struct<int_in_inner_record1:int,string_in_inner_record1:string> from deserializer
enum1   string  from deserializer
array1  array<string>   from deserializer
map1    map<string,string>      from deserializer
union1  uniontype<float,boolean,string> from deserializer
fixed1  binary  from deserializer
null1   void    from deserializer
unionnullint    int     from deserializer
bytes1  binary  from deserializer

Writing tables to Avro files

Hive 0.14 and later
In Hive versions 0.14 and later, you do not need to create the Avro schema manually. The procedure shown above to save a table as an Avro file reduces to just a DDL statement followed by an insert into the table.
CREATE TABLE as_avro(string1 STRING,
                     int1 INT,
                     tinyint1 TINYINT,
                     smallint1 SMALLINT,
                     bigint1 BIGINT,
                     boolean1 BOOLEAN,
                     float1 FLOAT,
                     double1 DOUBLE,
                     list1 ARRAY<STRING>,
                     map1 MAP<STRING,INT>,
                     struct1 STRUCT<sint:INT,sboolean:BOOLEAN,sstring:STRING>,
                     union1 uniontype<FLOAT, BOOLEAN, STRING>,
                     enum1 STRING,
                     nullableint INT,
                     bytes1 BINARY,
                     fixed1 BINARY)
STORED AS AVRO;
INSERT OVERWRITE TABLE as_avro SELECT * FROM test_serializer;

Avro file extension
The files that are written by the Hive job are valid Avro files, however, MapReduce doesn't add the standard .avro extension. If you copy these files out, you'll likely want to rename them with .avro.
Specifying the Avro schema for a table
There are three ways to provide the reader schema for an Avro table, all of which involve parameters to the serde. As the schema evolves, you can update these values by updating the parameters in the table.
Use avro.schema.url
Specifies a URL to access the schema from. For http schemas, this works for testing and small-scale clusters, but as the schema will be accessed at least once from each task in the job, this can quickly turn the job into a DDOS attack against the URL provider (a web server, for instance). Use caution when using this parameter for anything other than testing.
The schema can also point to a location on HDFS, for instance: hdfs://your-nn:9000/path/to/avsc/file. The AvroSerde will then read the file from HDFS, which should provide resiliency against many reads at once. Note that the serde will read this file from every mapper, so it's a good idea to turn the replication of the schema file to a high value to provide good locality for the readers. The schema file itself should be relatively small, so this does not add a significant amount of overhead to the process.
Use schema.literal and embed the schema in the create statement
You can embed the schema directly into the create statement. This works if the schema doesn't have any single quotes (or they are appropriately escaped), as Hive uses this to define the parameter value. For instance:
CREATE TABLE embedded
  COMMENT "just drop the schema right into the HQL"
  ROW FORMAT SERDE
  'org.apache.hadoop.hive.serde2.avro.AvroSerDe'
  STORED AS INPUTFORMAT
  'org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat'
  OUTPUTFORMAT
  'org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat'
  TBLPROPERTIES (
    'avro.schema.literal'='{
      "namespace": "com.howdy",
      "name": "some_schema",
      "type": "record",
      "fields": [ { "name":"string1","type":"string"}]
    }');
Note that the value is enclosed in single quotes and just pasted into the create statement.
Use avro.schema.literal and pass the schema into the script
Hive can do simple variable substitution and you can pass the schema embedded in a variable to the script. Note that to do this, the schema must be completely escaped (carriage returns converted to \n, tabs to \t, quotes escaped, etc). An example:
set hiveconf:schema;
DROP TABLE example;
CREATE TABLE example
  ROW FORMAT SERDE
  'org.apache.hadoop.hive.serde2.avro.AvroSerDe'
  STORED AS INPUTFORMAT
  'org.apache.hadoop.hive.ql.io.avro.AvroContainerInputFormat'
  OUTPUTFORMAT
  'org.apache.hadoop.hive.ql.io.avro.AvroContainerOutputFormat'
  TBLPROPERTIES (
    'avro.schema.literal'='${hiveconf:schema}');
To execute this script file, assuming $SCHEMA has been defined to be the escaped schema value:
hive --hiveconf schema="${SCHEMA}" -f your_script_file.sql
Note that $SCHEMA is interpolated into the quotes to correctly handle spaces within the schema.
Use none to ignore either avro.schema.literal or avro.schema.url
Hive does not provide an easy way to unset or remove a property. If you wish to switch from using URL or schema to the other, set the to-be-ignored value to none and the AvroSerde will treat it as if it were not set.


### CTAS Issue with JSON SerDe
Using the Hive CREATE TABLE ... AS SELECT command with a JSON SerDe results in a table that has column headers such as "_col0", which can be read by HCatalog or Hive but cannot be easily read by external users. To avoid this issue, create the table in two steps instead of using CTAS:
CREATE TABLE ...
INSERT OVERWRITE TABLE ... SELECT ...

## Custom serde
