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

The recurrence relation is:

$$T(n) = T(n/3) + T(2n/3) + \Theta(n)$$

Base case: $T(1) = \Theta(1)$.

The algorithm divides the array into two parts. The first
part contains one-third of the elements, while the second
part contains the remaining two-thirds. The combining step
takes linear time because the loop goes through every
element in the array.

### Part B: Visualizing Algorithm Behavior
Without solving the recurrence, look closely at the recursive calls and the combine step. If you were to draw the recursion tree for this algorithm, what would it look like? Is the tree balanced or skewed? 

The recursion tree would be unbalanced because the algorithm
does not divide the array into equal parts. The left side
has fewer elements, so it reaches the base case faster than
the right side. Each node also does linear work based on
the size of its subproblem.

---

## Exercise 2: Unrolling the Recursion Tree

Suppose you have developed a new divide-and-conquer algorithm. You analyze your pseudocode and determine that its worst-case running time $T(n)$ satisfies the following recurrence relation:

$$T(n) \leq 3T\left(\frac{n}{2}\right) + cn^2 \quad \text{for } n > 1$$
$$T(1) \leq c$$

where $c$ is a positive constant. 

### Part C: Recursion Tree Analysis

Analyze the running time of the algorithm by unrolling the recursion tree. Specifically, answer the following questions:

1. **Root Work:** The root does $cn^2$ work because it
   handles the entire input of size $n$.

2. **First Levels:** At level 1, the problem is divided
   into three subproblems of size $n/2$. Therefore, the
   total work is:

   $$3c(n/2)^2 = \frac{3}{4}cn^2$$

   At level 2, there are nine subproblems of size $n/4$.
   The total work is:

   $$9c(n/4)^2 = \frac{9}{16}cn^2$$

3. **Tree Height:** The input size is divided by two at
   each level until it reaches the base case. Therefore,

   $$\frac{n}{2^i} = 1$$

   The height of the tree is $\log_2 n$.

Also:

At level $i$, there are $3^i$ subproblems, each with
a size of $n/2^i$. Multiplying the number of subproblems
by the work done in each gives:

$$3^ic(n/2^i)^2 = cn^2(3/4)^i$$

## Exercise 3: Reflection

1. During lecture last week, we looked at two different divide-and-conquer algorithms. What were these algorithms and what problems did they solve?

The two algorithms we discussed in class were Merge Sort
and Nearest Neighbors. Merge Sort is used to sort elements
by dividing them into smaller parts and merging them back
together. Nearest Neighbors is used to find the two points
with the smallest distance between them.


2. How do the algorithms you analyzed in this lab compare to the ones we discussed in lecture? Specifically, each algorithm differs from the two we discussed in lecture in one important way. For each of the algorithms above, what is that difference?

The Mystery algorithm is different because it divides the
input into two unequal parts, while the algorithms we
discussed in class divide the input into two roughly
equal parts.

The algorithm in Exercise 2 is different because it makes
three recursive calls instead of two. It also does
quadratic work in the combining step, while Merge Sort
and Nearest Neighbors use linear combining steps.