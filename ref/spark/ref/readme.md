#### To read data 
val sample_07 = sc.textFile("s3a://s3-to-ec2/sample_07.csv")
var temp = sqlContext.read.format("com.databricks.spark.csv").option("header", "true").load("/user/hdfs/test.csv") 


#### To write data

df.write.format("parquet").partitionBy('..').saveAsTable(...)

or

df.write.format("parquet").partitionBy('...').insertInto(...)

#### To run queries

#### To add column

#### To register as temp table


####


####

####


####


####


####



####

####

## Read a text file in Amazon S3:
scala> val sample_07 = sc.textFile("s3a://s3-to-ec2/sample_07.csv")

Map lines into columns:

scala> import org.apache.spark.sql.Row

scala> val rdd_07 = sample_07.map(_.split('\t')).map(e ⇒ Row(e(0), e(1), e(2).trim.toInt, e(3).trim.toInt))

Create a schema and apply to the RDD to create a DataFrame:

scala> import org.apache.spark.sql.types.{StructType, StructField, StringType, IntegerType};

scala> val schema = StructType(Array(
  StructField("code",StringType,false),
  StructField("description",StringType,false),
  StructField("total_emp",IntegerType,false),
  StructField("salary",IntegerType,false)))

scala> val df_07 = sqlContext.createDataFrame(rdd_07,schema)

Write DataFrame to a Parquet file:

scala> df_07.write.parquet("s3a://s3-to-ec2/sample_07.parquet")
