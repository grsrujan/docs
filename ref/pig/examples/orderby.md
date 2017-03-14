To arrange a relation in a sorted order based on one or more fields (ascending or descending).
####Syntax
alias = ORDER alias BY { * [ASC|DESC] | field_alias [ASC|DESC] [, field_alias [ASC|DESC] …] } [PARALLEL n];
</br>
drivers =  LOAD '/user/maria_dev/drivers.csv' USING PigStorage(',')
AS (driverId:int, name:chararray, ssn:chararray,                 
location:chararray, certified:chararray, wage_plan:chararray);<br>
ordered_data = ORDER drivers BY name asc;<br>
DUMP ordered_data;
