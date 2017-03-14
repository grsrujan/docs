To split a single relation into two or more relations.
####Syntax
SPLIT alias INTO alias IF expression, alias IF expression [, alias IF expression …] [, alias OTHERWISE];

Example
In this example relation A is split into three relations, X, Y, and Z.

A = LOAD 'data' AS (f1:int,f2:int,f3:int);

DUMP A;                
(1,2,3)
(4,5,6)
(7,8,9)        

SPLIT A INTO X IF f1<7, Y IF f2==5, Z IF (f3<6 OR f3>6);

DUMP X;
(1,2,3)
(4,5,6)

DUMP Y;
(4,5,6)

DUMP Z;
(1,2,3)
(7,8,9)

Example
In this example, the SPLIT and FILTER statements are essentially equivalent. However, because SPLIT is implemented as "split the data stream and then apply filters" the SPLIT statement is more expensive than the FILTER statement because Pig needs to filter and store two data streams.

SPLIT input_var INTO output_var IF (field1 is not null), ignored_var IF (field1 is null);  
-- where ignored_var is not used elsewhere
   
output_var = FILTER input_var BY (field1 is not null);
