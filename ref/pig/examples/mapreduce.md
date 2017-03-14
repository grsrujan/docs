Executes native MapReduce jobs inside a Pig script.
</br>
alias1 = MAPREDUCE 'mr.jar' STORE alias2 INTO 'inputLocation' USING storeFunc LOAD 'outputLocation' USING loadFunc AS schema [`params, ... `];
</br>
A = LOAD 'WordcountInput.txt';</br>
B = MAPREDUCE 'wordcount.jar' STORE A INTO 'inputDir' LOAD 'outputDir' 
    AS (word:chararray, count: int) `org.myorg.WordCount inputDir outputDir`;
