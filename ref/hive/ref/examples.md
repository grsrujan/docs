####CREATE TABLE
create table managed_table(dummy string);

####LOADING DATA FROM HDFS TO TABLE
load data inpath'/user/srujan/data.txt' into table managed_table;

create external table external_table(dummy string)
location '/user/srujan/external_table';

load data inpath'/user/tom/data.txt' into table external_table;


data will be in hive warehouse directory


/home/hive/warehouse/employee/part-m-0000,part-m-0001


partitions work with only internal tables not external tables

create table employee(eno int,name string,sal int,age int,city string,country string);

load data local infile 'location' into table employee;

hdfs://home/hive/warehouse/employee/emp1.txt,emp2.txt,emp3.txt

data is already there in hdfs 

e.g:- hdfs://projectdata/emp/emp1.txt,emp2.txt,emp3.txt

create table in hive referring to the existing data in hdfs



create external table employee(eno int ,name string) row format delimited fields terminated by '|'location '/projectdata/emp';

/home/hive/warehouse/employee/part-m-0000,part-m-0001,


in order to create parttitions we will use

create table employee(eno int,name string,sal int,age int) partitioned by(country string,city string);

row format delimited fields terminated by '|';


insert overwrite table target select col1,col2 from source;


overwrite keyword replaces the contents in HDFS directory

dynmic partition insser:
insert overwrite table target partition(dt) select col1,col2,dt from source;



atering or dropping tables

alter table source rename to target;

in addition to updating the table metadata alter table moves the underlying table directory so that it reflects the new name.

to add new column

alter table target add columns (col string);

the new column col is added after the existing (non partition) columns.

drop table -deletes data and metadata.

if we want to delete all the data in a table but want to keep the table definition(like DELETE or TRUNCATE in MYSQL) then you can simply delete the data files for example

dfs -rmr /user/hive/warehouse/mytable;


ls

hive

to quit from hive just type quit

cloudera managger portno

localhost:7180/cmf/login

username:cloudera
password: edureka


show databases;

show tables;

drop table tablename;

sqoop hive import

mysql-uroot  (in seperate window)

show databases;

use practice;


show tables;


sqoop import --connect jdbc:mysql://localhost/sqooppractice --username root --table employees --hive-import(in separate window)

hadoop fs -ls /user/hive/warehouse

hadoop fs -cat /user/hive/warehouse/employees/part-m-00003


sqoop import --connect jdbc:mysql://localhost/sqooppractice --username root --table customers --hive-import(in separate window)


hive--> show tables;



loading data from external file to hive

create table order_details(order_id int,prod_id int) row format delimited fileds terminated by '\t';

load data local inpath '/home/cloudera/desktop/scriptts/analyst/static_data/oredr_details/part-m-00000' into table order_details;


hive-->create external table employees(emp_id string,fname string,lname string,address string,city sring,state string,zipcode string,desig string,emailid string,isactive string,salary int) row format delimited fields terminated by '\t' location '/static_data/employees';

desc employees;

select * from employees limit 4;


####hwo to create internal table from external table 


hive---> create table employee_stage as select emp_id,fname,lname,city,state,zipcode,emailid,salary from employees;

show tables;

describe employee_stage;

drop table employees;


create table employees(emp_id string,fname string,lname string,address string,zipcode string,desig string,emailid string,isactive string,salary int) partitioned by(state string,city sring) row format delimited fields terminated by '\t';



to load data into empty table use the following query
insert overwrite table employees_part partition(state,city) select emp_id,fname,lname,address,zipcode,designation,emailid,isactive,salary,state,city from employees;
