The 3 core methods of a reducer are –
1)setup () – This method of the reducer is used for configuring various parameters like the input data size, distributed cache, heap size, etc.
Function Definition- public void setup (context)
2)reduce () it is heart of the reducer which is called once per key with the associated reduce task.
Function Definition -public void reduce (Key,Value,context)
3)cleanup () - This method is called only once at the end of reduce task for clearing all the temporary files.
Function Definition -public void cleanup (context)


 Subclasses:
	ChainReducer, FieldSelectionReducer, IntSumReducer, LongSumReducer, ValueAggregatorCombiner, ValueAggregatorReducer, WrappedReducer

From <https://hadoop.apache.org/docs/r2.4.1/api/org/apache/hadoop/mapreduce/Reducer.html> 
