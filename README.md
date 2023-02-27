# Optimal Tuple Combination

This git is a result of an interesting task I recently saw. The task description
is the following:

<h2>Problem description</h2>	

You are given a set of variable tuples 
$X = \{(x_1^1, x_2^1, .., x_n^1), ..., (x_1^m, x_2^m, .., x_n^m)\}$. <br>
Further you have given a "requirement" for every variable $O = o_1, ..., o_n$, which is either maximize or minimize it. An example could be having a set of 3-tuples, where $x_1$ should be minimized, $x_2$ maximized, and $x_3$ minimized.<br>
The objective is to select an optimal combination $X_i$, such that each requirement is optimized.

<h2>Problem solution</h2>	

This problem can be solved as follows:

We have a two level optimization problem. The first level is the individual level, where each value should be minimized/maximized by itself. The individual level alone would be easy, as just the min/max value could be selected. The problem is, the values are not comparable between each other and do not give a general objective function. They are not comparable as nothing about the domain or distribution of the values is known, just the value itself. In an unweighted setting, comparing the plain values will inevitably lead to putting more weight onto some variables. <br>
To overcome this issue, we create a ranking of the values of each variable. For minimizing a value, the smallest value will get the rank $1$ and the largest the value $|X|$. For maximizing a value, take the negative rank. I.e., for the smallest value the rank $-1$ and the largest $-|X|$.<br>
Now, a general objective function which compares values of each tuple independently is missing. We define this objective function as minimizing the sum of each transformed tuple.

<h2>Sketch of proof</h2>	

For an unweighted optimization, this solution is optimal: 

Take the problem of optimizing 
$X = \{(x_1^1, x_2^1, .., x_n^1), ..., (x_1^m, x_2^m, .., x_n^m)\}$ with the objectives
$o_1, ..., o_n$. 

Take the solution we obtained by applying the procedure formulated above and denote this solution with 
$X^{\*} = (x_1^{\*}, x_2^{\*}, .., x_n^{\*})$.<br> 
Consider the case where the solution we obtained is not optimal, which means there is an 
$X^{'}$ for which holds for at least one 
$o_i$: 
$o_i(x_i^{'}) > o_i(x_i^{\*})$, where ">" refers to a better value w.r.t the optimal function (e.g. a bigger maximum). Then, in our case, the transformed ranks would correspond to this, meaning the rank of 
$x_i^{'}$ is by definition lower than 
$x_i^{\*}$. 

This contradicts our initial definition, that
$X^\*$ is the lowest obtained sum value, as 
$X^{'}$ would have a lower one ↯

<h2>Example notebook</h2>	
The optimizer.ipynb notebook shows an implementation of an example of the problem for 3 variables A, B, C. Two variables, A and C, should be minimized and B should be maximized.
The implementation uses Pandas/numpy and with basic sorting operations.

I tested it up to 20 000 000 different tuples and it ran in 30 seconds on my laptop (Intel I5 1035G4)

Generally, we sort n+1 times. Each variable once and then the sum of the variables. Pandas uses numpys quicksort algorithm, which runs in 
$O(m \* log(m))$, giving an overall runtime of 
$O(n+1 \* (m \* log(m)))$ (sums and weightings are linear, which doesn't affect the overall runtime class)
