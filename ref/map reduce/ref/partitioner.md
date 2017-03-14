####Partitioning 
The process that determines which intermediate keys and value will be received by each reducer instance is referred to as partitioning. The destination partition is same for any key irrespective of the mapper instance that generated it.

####Custom Partitioner
To do
##### steps to write custom partitioner
• A new class must be created that extends the pre-defined Partitioner Class.
• getPartition method of the Partitioner class must be overridden.
The custom partitioner to the job can be added as a config file in the wrapper which runs Hadoop MapReduce or the custom partitioner can be added to the job by using the set method of the partitioner class.
