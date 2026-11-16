# Lab 09: Exploring D&C Algorithms

**Instructions:** Work in small groups to complete the following exercises. Try sketching out your work on the whiteboards or scratch paper. Everyone should submit their own answers to the exercises, but you are encouraged to discuss your reasoning with your group members.

Name(s): Marshall Obeng Benne 

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

The algorithm makes recursive calls on about $n/3$ and $2n/3$ elements, then does a linear pass through the array. Thus, for $n > 1$,

$$T(n) = T(n/3) + T(2n/3) + \Theta(n), \qquad T(1) = \Theta(1).$$

### Part B: Visualizing Algorithm Behavior
Without solving the recurrence, look closely at the recursive calls and the combine step. If you were to draw the recursion tree for this algorithm, what would it look like? Is the tree balanced or skewed? 

Each node has two children, but they are different sizes: one gets about one-third of the elements and the other gets about two-thirds. The tree is therefore unbalanced rather than evenly split. The one-third branches reach the base case sooner, while repeatedly following the larger branch takes more levels. Each node also does linear work in the size of its subproblem.

---

## Exercise 2: Unrolling the Recursion Tree

Suppose you have developed a new divide-and-conquer algorithm. You analyze your pseudocode and determine that its worst-case running time $T(n)$ satisfies the following recurrence relation:

$$T(n) \leq 3T\left(\frac{n}{2}\right) + cn^2 \quad \text{for } n > 1$$
$$T(1) \leq c$$

where $c$ is a positive constant. 

### Part C: Recursion Tree Analysis

Analyze the running time of the algorithm by unrolling the recursion tree. Specifically, answer the following questions:

1. **Root Work:** The root handles an input of size $n$, so its work is $cn^2$.
2. **First Levels:** At level $1$, there are 3 subproblems of size $n/2$, for a total of $3c(n/2)^2 = \frac{3}{4}cn^2$. At level $2$, there are 9 subproblems of size $n/4$, for a total of $9c(n/4)^2 = \frac{9}{16}cn^2$.
3. **Tree Height:** After $i$ levels, each subproblem has size $n/2^i$. Setting this equal to 1 gives $i = \log_2 n$, so the height is $\log_2 n$ (up to rounding when $n$ is not a power of 2).

**Bonus!** At level $i$, there are $3^i$ subproblems, each of size $n/2^i$. The total work at that level is

$$3^i c\left(\frac{n}{2^i}\right)^2 = cn^2\left(\frac{3}{4}\right)^i.$$

---

## Exercise 3: Reflection

1. During lecture last week, we looked at two different divide-and-conquer algorithms. What were these algorithms and what problems did they solve?

The two examples were merge sort, which sorts a list by sorting its halves and merging them, and binary search, which finds a target in a sorted list by repeatedly narrowing the search to one half.


2. How do the algorithms you analyzed in this lab compare to the ones we discussed in lecture? Specifically, each algorithm differs from the two we discussed in lecture in one important way. For each of the algorithms above, what is that difference?

`Mystery` makes recursive calls on two unequal parts (about one-third and two-thirds), unlike merge sort's equal halves. It also recurses on both parts, unlike binary search, which follows just one half.

The recurrence in Exercise 2 makes three recursive calls on half-size inputs and has quadratic work at each node. In contrast, merge sort makes two such calls and does linear combine work, while binary search makes one such call and does constant work outside the call.
