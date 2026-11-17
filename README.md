# Lab 09: Exploring D&C Algorithms

**Instructions:** Work in small groups to complete the following exercises. Try sketching out your work on the whiteboards or scratch paper. Everyone should submit their own answers to the exercises, but you are encouraged to discuss your reasoning with your group members.

Name(s): Krish Yendluri

---

## Exercise 1: Recurrence Extraction

Consider the following divide-and-conquer algorithm, which takes an array $A$ of $n$ numbers as input and returns a single number. 

```text
Procedure Mystery(A, n)
    if n <= 1:
        return A[1]                              // Base case
        
    k = floor(n / 3)                             // Find the one-third point
    
    left = Mystery(A[1..k], k)                   // Recur on the left third
    right = Mystery(A[k+1..n], n - k)            // Recur on the rest
    
    total = 0 
    for i = 1 to n:
        total = total + (A[i] * left * right)    // Combine results
        
    return total
```

### Part A: The Recurrence Relation
Let $T(n)$ be the running time of `Mystery` on an input of size $n$. Write down a recurrence relation for the worst-case running time $T(n)$. You may assume $n$ is a power of 3 for simplicity. *(Note: You do not need to solve the recurrence relation yet, just formulate it).*

**Answer:**

For $n > 1$, the algorithm makes one recursive call on an input of size
$n/3$ and another on an input of size $2n/3$. The loop takes linear time,
so the recurrence is

$$
T(n) = T\left(\frac{n}{3}\right) +
T\left(\frac{2n}{3}\right) + cn + d,
$$

for positive constants $c$ and $d$, with base case $T(1) = \Theta(1)$.

### Part B: Visualizing Algorithm Behavior
Without solving the recurrence, look closely at the recursive calls and the combine step. If you were to draw the recursion tree for this algorithm, what would it look like? Is the tree balanced or skewed? 

**Answer:**

Each internal node has two children, but the children have sizes $n/3$ and
$2n/3$, not equal sizes. Therefore, the tree is unbalanced (skewed toward
the $2n/3$ child). The longest branch repeatedly follows the larger child,
so it reaches the base case more slowly than a branch that repeatedly follows
the $n/3$ child. The tree is not a perfectly balanced binary recursion tree.

---

## Exercise 2: Unrolling the Recursion Tree

Suppose you have developed a new divide-and-conquer algorithm. You analyze your pseudocode and determine that its worst-case running time $T(n)$ satisfies the following recurrence relation:

$$T(n) \leq 3T\left(\frac{n}{2}\right) + cn^2 \quad \text{for } n > 1$$
$$T(1) \leq c$$

where $c$ is a positive constant. 

### Part C: Recursion Tree Analysis

Analyze the running time of the algorithm by unrolling the recursion tree. Specifically, answer the following questions:
1. **Root Work:** Find the amount of work done at the root level (level $0$) of the recursion tree.
2. **First Levels:** Find the amount of work done at level $1$ and level $2$ of the recursion tree.
3. **Tree Height:** Find the height of the recursion tree (at what level does the input size reach $1$?).

**Bonus!** Find a general formula for the total work done at level $i$ of the recursion tree.

**Answer:**

1. At level $0$, there is one problem of size $n$, so the root work is
   $cn^2$.
2. At level $1$, there are $3$ problems of size $n/2$. Thus, the work is
   \[
   3c\left(\frac{n}{2}\right)^2 = \frac{3}{4}cn^2.
   \]
   At level $2$, there are $3^2=9$ problems of size $n/4$. Thus, the work is
   \[
   9c\left(\frac{n}{4}\right)^2 = \frac{9}{16}cn^2.
   \]
3. After level $i$, each subproblem has size $n/2^i$. Set this equal to $1$:
   \[
   \frac{n}{2^i}=1 \quad\Longrightarrow\quad i=\log_2 n.
   \]
   Therefore, the recursion tree has height $\log_2 n$ (with the leaves at
   level $\log_2 n$).

   In general, level $i$ contains $3^i$ subproblems, each doing
   $c(n/2^i)^2$ work. Therefore, the work at level $i$ is
   \[
   3^i c\left(\frac{n}{2^i}\right)^2
   = cn^2\left(\frac{3}{4}\right)^i.
   \]

---

## Exercise 3: Reflection

1. During lecture last week, we looked at two different divide-and-conquer algorithms. What were these algorithms and what problems did they solve?

**Answer:**

The two algorithms were **Merge Sort**, which sorts an array by recursively
sorting two halves and merging them, and **Quick Sort**, which sorts an array
by partitioning it around a pivot and recursively sorting the resulting
subarrays.




2. How do the algorithms you analyzed in this lab compare to the ones we discussed in lecture? Specifically, each algorithm differs from the two we discussed in lecture in one important way. For each of the algorithms above, what is that difference?

**Answer:**

`Mystery` is similar to the sorting algorithms because it recursively breaks
an array into smaller subproblems, but it splits the input into unequal
pieces of sizes $n/3$ and $2n/3$ rather than the equal halves used by Merge
Sort (and the usual balanced Quick Sort analysis). Its combine step also
does a linear scan instead of merging sorted lists or partitioning around a
pivot.

The recurrence in Exercise 2 differs because it creates **three** recursive
subproblems of size $n/2$ and has a quadratic combine cost, $cn^2$. Merge
Sort has two half-sized subproblems and linear combine work, while Quick Sort
has two partition-dependent subproblems and usually linear partition work.