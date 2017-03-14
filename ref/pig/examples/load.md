To Load the data from the file system (local/HDFS) into a relation.</br>
The two LOAD statements are equivalent. Note that, because no schema is specified, the fields are not named and all fields default to type bytearray.</br>
A = LOAD 'myfile.txt';

A = LOAD 'myfile.txt' USING PigStorage('\t');
</br>
student = LOAD 'hdfs://localhost:9000/pig_data/student_data.txt' 
   USING PigStorage(',')
   as ( id:int, firstname:chararray, lastname:chararray, phone:chararray, 
   city:chararray );
