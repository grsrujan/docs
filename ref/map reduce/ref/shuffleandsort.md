###contents

####Shuffle Phase
Once the first map tasks are completed, the nodes continue to perform several other map tasks and also exchange the intermediate outputs with the reducers as required. This process of moving the intermediate outputs of map tasks to the reducer is referred to as Shuffling.

####Sort Phase
Hadoop MapReduce automatically sorts the set of intermediate keys on a single node before they are given as input to the reducer.
