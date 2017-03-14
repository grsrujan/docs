Limits the number of output tuples.

#####Syntax
alias = LIMIT alias  n;</br>
n can be constant/scalar
a = load 'a.txt';
b = group a all;
c = foreach b generate COUNT(a) as sum;
d = order a by $0;
e = limit d c.sum/100;


A = LOAD 'data' AS (a1:int,a2:int,a3:int);
X = LIMIT A 3;
