# Advanced Data Structures & Algorithms

# Module 1: AVL Tree

# Lesson 1: Why AVL Tree Exists

---

## Learning Objectives

By the end of this lesson, you will understand:

* Why Binary Search Trees fail in some cases
* The concept of tree height
* Why balancing is necessary
* The motivation behind AVL Trees
* The Balance Factor concept
* How AVL Trees guarantee efficient operations

---

# 1. Historical Background

Before AVL Trees were invented, programmers used ordinary Binary Search Trees (BSTs).

The theoretical complexity of BST operations looks excellent:

| Operation | Time Complexity |
| --------- | --------------- |
| Search    | O(log n)        |
| Insert    | O(log n)        |
| Delete    | O(log n)        |

At first glance, BST appears to be the perfect data structure.

However, there is a hidden assumption:

> The tree remains balanced.

Unfortunately, BST does not guarantee balance.

This creates a major problem.

---

# 2. The Fundamental Problem with BST

Consider inserting the following values into an empty BST:

```text
1 2 3 4 5 6 7 8
```

Let's build the tree step by step.

## After inserting 1

```text
1
```

---

## After inserting 2

```text
1
 \
  2
```

---

## After inserting 3

```text
1
 \
  2
   \
    3
```

---

## After inserting 4

```text
1
 \
  2
   \
    3
     \
      4
```

---

## Final Tree

```text
1
 \
  2
   \
    3
     \
      4
       \
        5
         \
          6
           \
            7
             \
              8
```

---

# 3. Is This Still a BST?

Yes.

Let's verify BST properties:

For every node:

* Left subtree contains smaller values
* Right subtree contains larger values

Example:

```text
1 < 2 < 3 < 4 < 5 < 6 < 7 < 8
```

Therefore:

✅ Valid BST

However...

❌ Efficient BST

---

# 4. Why Searching Becomes Slow

Suppose we search for:

```text
8
```

Path followed:

```text
1 → 2 → 3 → 4 → 5 → 6 → 7 → 8
```

Comparisons performed:

```text
8
```

For n nodes:

```text
n comparisons
```

Therefore:

```math
T(n) = O(n)
```

The BST has effectively become a linked list.

---

# 5. Real-World Analogy

Imagine a library.

## Balanced Library

```text
Floor 1
Floor 2
Floor 3
Floor 4
```

Any floor can be reached quickly.

---

## Skewed Library

```text
Room
 └── Room
      └── Room
           └── Room
                └── Room
```

To reach the last room, every room must be crossed.

This is exactly how a skewed BST behaves.

---

# 6. Core Principle of Binary Search

Binary Search is efficient because it eliminates half the search space at every step.

Example:

```text
1000 elements

1000
500
250
125
63
32
16
8
4
2
1
```

Number of operations:

```math
\log_2 n
```

This is why Binary Search runs in:

```math
O(\log n)
```

---

# 7. BST and Binary Search Connection

A balanced BST behaves similarly to Binary Search.

Example:

```text
          50
        /    \
      25      75
     /  \    /  \
   10   40 60   90
```

Search for:

```text
90
```

Comparisons:

```text
50
75
90
```

Only three comparisons are needed.

This is why balanced trees are powerful.

---

# 8. The Most Important Observation

Search complexity depends on:

```math
Height(Tree)
```

not on the total number of nodes.

Many students misunderstand this.

---

## Example 1

```text
1
 \
 2
  \
   3
    \
     4
```

Nodes = 4

Height = 4

---

## Example 2

```text
    2
   / \
  1   3
       \
        4
```

Nodes = 4

Height = 3

---

Same number of nodes.

Different heights.

Different performance.

---

# 9. Why Height Matters

Searching always follows exactly one path.

If tree height is:

```math
h
```

Maximum comparisons:

```math
h
```

Therefore:

```math
Search = O(h)
```

This formula is extremely important.

For every tree data structure:

```math
Search = O(Height)
```

---

# 10. What Is the Ideal Height?

Suppose:

```text
n = 15
```

Perfectly balanced tree:

```text
               x
            /     \
          x         x
        /  \      /   \
       x    x    x     x
      /\   /\   /\    /\
```

Level-wise nodes:

```text
1
2
4
8
```

Total:

```text
15 Nodes
```

Height:

```text
4
```

Observe:

```math
15 \approx 2^4
```

Thus:

```math
h = \log_2 n
```

This is the ideal height.

---

# 11. The Goal of Computer Scientists

Researchers wanted a BST where:

```math
h = O(\log n)
```

always.

Not sometimes.

Not usually.

Always.

This requirement led to the invention of AVL Trees.

---

# 12. Birth of AVL Tree

Researchers asked:

> Can we automatically rebalance a BST after insertions and deletions?

The answer was:

**AVL Tree**

AVL stands for:

* Adelson-Velsky
* Landis

The inventors of the data structure.

AVL became the first self-balancing Binary Search Tree.

---

# 13. Key Idea Behind AVL Trees

Do not allow one side of the tree to become too tall.

---

## Balanced Example

```text
      30
     /  \
   20   40
```

Height Difference:

```text
0
```

Balanced.

---

## Still Balanced

```text
      30
     /
   20
```

Height Difference:

```text
1
```

Balanced.

---

## Unbalanced

```text
      30
     /
   20
   /
 10
```

Height Difference:

```text
2
```

Unbalanced.

AVL Tree will immediately fix this situation.

---

# 14. Balance Factor

AVL Trees measure imbalance using:

```math
BF = Height(Left) - Height(Right)
```

This is called the **Balance Factor**.

---

## Example 1

```text
      30
     /  \
   20   40
```

```math
BF = 1 - 1 = 0
```

Balanced.

---

## Example 2

```text
      30
     /
   20
```

```math
BF = 1 - 0 = 1
```

Balanced.

---

## Example 3

```text
      30
     /
   20
   /
 10
```

```math
BF = 2 - 0 = 2
```

Not Balanced.

AVL must rebalance.

---

# 15. AVL Property

Every node in an AVL Tree must satisfy:

```math
-1 \le BF \le 1
```

Allowed:

```text
-1
 0
+1
```

Not Allowed:

```text
-2
+2
```

---

# Key Takeaways

* BST performance depends on height.
* Height can become O(n).
* Skewed BST behaves like a linked list.
* Search complexity is O(height).
* Balanced trees maintain logarithmic height.
* AVL Tree is a self-balancing BST.
* AVL uses Balance Factor.
* Every AVL node must satisfy:

```math
-1 \le BF \le 1
```

---

# Trainer Notes

When teaching students:

1. Start with BST.
2. Demonstrate skewed BST formation.
3. Show degradation from O(log n) to O(n).
4. Introduce the concept of height.
5. Explain why balancing matters.
6. Only then introduce AVL Trees.

Students understand AVL much faster when they first experience the failure of ordinary BSTs.

---

# Conceptual Questions

1. Why can a BST become O(n)?
2. Why is search complexity equal to tree height?
3. Why is a balanced BST efficient?
4. What problem does AVL Tree solve?
5. What is the Balance Factor?
6. Why are only -1, 0, and +1 allowed?
7. How does AVL maintain logarithmic height?

---

# Coding Exercise

Implement a BST and insert:

```text
1 2 3 4 5 6 7 8
```

Then:

1. Compute tree height.
2. Search for 8.
3. Count comparisons.
4. Compare with Binary Search.

Observe how the BST degrades into a linked list.
