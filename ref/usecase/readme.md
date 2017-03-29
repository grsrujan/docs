## Contents

- [analyzing-fantasy-sports-using-apache-spark-and-sql](https://blog.cloudera.com/blog/2016/06/how-to-analyze-fantasy-sports-using-apache-spark-and-sql/)
- [Impala Usage at allstate](https://blog.cloudera.com/blog/2014/05/using-impala-at-scale-at-allstate/)
- [tuning-spark-streaming-app-for-real-time-processing-with-apache-kafka](https://blog.cloudera.com/blog/2016/01/how-cigna-tuned-its-spark-streaming-app-for-real-time-processing-with-apache-kafka/)
- [hue usage at ibibo](https://blog.cloudera.com/blog/2014/04/hue-flies-high-at-goibibo/)
- [Choosing table based on data](https://www.mapr.com/blog/what-kind-hive-table-best-your-data)
- [real-time-credit-card-fraud-detection-apache-spark-and-event-streaming](https://www.mapr.com/blog/real-time-credit-card-fraud-detection-apache-spark-and-event-streaming)
- [monitoring-real-time-uber-data-part1](https://www.mapr.com/blog/monitoring-real-time-uber-data-using-spark-machine-learning-streaming-and-kafka-api-part-1)
- [monitoring-real-time-uber-data-part2](https://www.mapr.com/blog/how-use-data-science-and-machine-learning-revolutionize-360%C2%B0-customer-views-part-2)
- [spark-streaming-hbase](https://www.mapr.com/blog/spark-streaming-hbase)
- [using-hive-and-pig-baseball-statistics](https://www.mapr.com/blog/using-hive-and-pig-baseball-statistics)
- [sample-programs-apache-kafka](https://www.mapr.com/blog/getting-started-sample-programs-apache-kafka-09)
- [near-real-time-data-processing](http://blog.cloudera.com/blog/2015/06/architectural-patterns-for-near-real-time-data-processing-with-apache-hadoop/)
- [improve-apache-hbase-performance-via-data-serialization-with-apache-avro](https://blog.cloudera.com/blog/2016/05/how-to-improve-apache-hbase-performance-via-data-serialization-with-apache-avro/)
- [web services with Flume](http://www.ibm.com/developerworks/library/bd-flumews/)
- [hive-streaming-with-storm](http://henning.kropponline.de/2015/01/24/hive-streaming-with-storm/)
- [MapReduce Jobs In Java](http://www.higherpass.com/java/Tutorials/Building-Hadoop-Mapreduce-Jobs-In-Java/)
- [Financial-hortonworks](http://hortonworks.com/solutions/financial-services/)
- [Financial-cloudera ](https://www.cloudera.com/solutions/financial-services.html)
- [Financial-mapr](https://www.mapr.com/solutions/industry/financial-services-use-cases)
- [Financial datameer](https://datameer-wp-production-origin.datameer.com/wp-content/uploads/2015/10/eBook-3-Top-BigData-UseCase-in-Financial-Services.pdf)
- [storm-integration-with-kafka](http://www.allprogrammingtutorials.com/tutorials/storm-integration-with-kafka.php)

# general use cases
- [Different ways to offload mainframe data to hadoop]()

In general, we will have three different data sources from mainframes: flat files, VSAM files and a DBMS such as DB2 or IMS.We can use following tools: <br>
#### Informatica BDE(Big Data Edition)<br>
#### Talend Enterprise Edition<br>
#### Syncsort-dmx(plugin)<br>
#### Sqoop(with following limitations)<br>
* Sqoop-mainframe can only handle Fixed length files, variable length files are not supported.
* Only folder structures like data can be downloaded i.e., PDS(partitioned data sets) and Fixed length GDGs as a whole.
* EBCIDIC to ASCII conversion return invalid data when the data has computational fields.<br>
- [custom scripts(cobal2hive)](https://rbheemana.github.io/Cobol-to-Hive/)<br>
#### converting vsam --flat files using IDCAMS utility and load into Hadoop<br>
#### zaloni bedrock<br>
#### veristorm<br>
- [setting passwordless connection](http://www.rebol.com/docs/ssh-auto-login.html)
- [Dealing with CDC(change data capture data in Hadoop)]()
- [How to handle dynamic schema changes in hive](https://hadoopist.wordpress.com/2015/07/08/how-to-handle-schema-changesevolutes-in-hive-like-column-additions-at-the-source-db/)
