####create job
 sqoop job --create myjob \
--import \
--connect jdbc:mysql://localhost/db \
--username root \
--table employee --m 1

####list of available jobs
sqoop job --list

####Inspect a job
 sqoop job --show myjob

####Execute Job
sqoop job --exec myjob
