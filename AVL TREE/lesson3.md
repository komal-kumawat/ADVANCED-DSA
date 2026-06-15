# Advanced Data Structures & Algorithms

# Module 3: AVL Rotations Deep Dive

# Lesson 3: Rotations - The Heart of AVL Trees

---

# Learning Objectives

By the end of this lesson, you will understand:

* Why rotations are needed
* What happens when AVL balance is violated
* The four types of AVL rotations
* LL Rotation
* RR Rotation
* LR Rotation
* RL Rotation
* Why rotations preserve BST properties
* Mathematical correctness of rotations
* How interviewers test rotations

---

# 1. Why Rotations Exist

In the previous lesson, we proved:

```math
Height = O(\log n)
```

only if the AVL property is maintained.

Recall:

```math
-1 \le BF \le 1
```

for every node.

But what happens after insertion?

Suppose we insert a node into an AVL Tree.

The Balance Factor of some ancestor may become:

```math
BF = +2
```

or

```math
BF = -2
```

Now the AVL property is violated.

We need a mechanism to restore balance.

That mechanism is called:

# Rotation

---

# 2. What is a Rotation?

A rotation is a local restructuring operation that:

1. Restores balance
2. Preserves BST property
3. Changes only a small part of the tree
4. Runs in O(1) time

Think of it like adjusting a tilted bookshelf.

Before:

```text
     /
    /
   /
```

After adjustment:

```text
   /\
```

The bookshelf remains organized but becomes stable.

AVL rotations work exactly the same way.

---

# 3. The Key Observation

When a node becomes unbalanced:

Only one small subtree needs fixing.

We do NOT rebuild the entire tree.

We only rotate around the first unbalanced node.

This is why AVL insertion remains efficient.

---

# 4. When Does Imbalance Occur?

After insertion:

Only ancestors of the inserted node can become unbalanced.

Example:

Insert:

```text
30
20
10
```

Tree:

```text
      30
     /
   20
   /
 10
```

Balance Factors:

```text
10 -> 0
20 -> +1
30 -> +2
```

Node 30 is unbalanced.

We must fix it.

---

# 5. Classification of Imbalance

There are only four possible cases.

| Case | Path        |
| ---- | ----------- |
| LL   | Left Left   |
| RR   | Right Right |
| LR   | Left Right  |
| RL   | Right Left  |

Every AVL insertion falls into exactly one of these four cases.

Memorizing this table is essential.

---

# 6. LL Rotation (Left-Left Case)

---

## How LL Happens

Insertion occurs in:

```text
Left subtree of Left child
```

Pattern:

```text
      Z
     /
    Y
   /
  X
```

Visualize the path:

```text
Z → Y → X
```

All moves are toward the left.

Hence:

```text
Left Left
```

---

## Example

Insert:

```text
30
20
10
```

Tree becomes:

```text
      30
     /
   20
   /
 10
```

---

## Balance Factors

Node 10:

```text
BF = 0
```

Node 20:

```text
BF = +1
```

Node 30:

```text
BF = +2
```

Violation detected.

---

# Right Rotation

We rotate around 30.

Before:

```text
      30
     /
   20
   /
 10
```

After:

```text
      20
     /  \
   10   30
```

Balanced again.

---

# Why It Works

Observe BST order before rotation:

```text
10 < 20 < 30
```

After rotation:

```text
10 < 20 < 30
```

Order remains unchanged.

Therefore BST property is preserved.

---

# LL Rule

Whenever:

```text
BF(Z)=+2
```

and

```text
BF(Y)>=0
```

Perform:

```text
Right Rotation
```

---

# 7. RR Rotation (Right-Right Case)

---

## How RR Happens

Insertion occurs in:

```text
Right subtree of Right child
```

Pattern:

```text
Z
 \
  Y
   \
    X
```

Path:

```text
Z → Y → X
```

All moves are toward the right.

Hence:

```text
Right Right
```

---

## Example

Insert:

```text
10
20
30
```

Tree:

```text
10
 \
 20
   \
   30
```

---

## Balance Factors

Node 10:

```text
BF = -2
```

Unbalanced.

---

# Left Rotation

Before:

```text
10
 \
 20
   \
   30
```

After:

```text
     20
    /  \
  10   30
```

Balanced.

---

# RR Rule

Whenever:

```text
BF(Z)=-2
```

and

```text
BF(Y)<=0
```

Perform:

```text
Left Rotation
```

---

# 8. LR Rotation (Left-Right Case)

---

## How LR Happens

Insertion occurs:

```text
Left child
   →
Right subtree
```

Pattern:

```text
      Z
     /
    Y
     \
      X
```

Path:

```text
Z → Left → Right
```

Hence:

```text
Left Right
```

---

# Example

Insert:

```text
30
10
20
```

Tree:

```text
      30
     /
   10
     \
      20
```

---

# Why Single Rotation Fails

Suppose we perform only right rotation.

Result:

```text
      10
        \
         30
        /
      20
```

Still unbalanced.

Therefore LL fix does not work.

---

# Solution

Two rotations are required.

---

## Step 1

Rotate Left at 10

Before:

```text
    10
      \
      20
```

After:

```text
      20
     /
   10
```

Entire tree:

```text
      30
     /
   20
   /
 10
```

---

## Step 2

Right Rotate at 30

Result:

```text
      20
     /  \
   10   30
```

Balanced.

---

# LR Rule

When:

```text
BF(Z)=+2
```

and

```text
BF(Y)=-1
```

Perform:

```text
Left Rotate(Y)
Right Rotate(Z)
```

---

# 9. RL Rotation (Right-Left Case)

---

## How RL Happens

Pattern:

```text
Z
 \
  Y
 /
X
```

Path:

```text
Z → Right → Left
```

Hence:

```text
Right Left
```

---

# Example

Insert:

```text
10
30
20
```

Tree:

```text
10
 \
 30
 /
20
```

---

# Why Single Rotation Fails

Performing only left rotation produces:

```text
      30
     /
   10
     \
     20
```

Still unbalanced.

Need double rotation.

---

# Step 1

Right Rotate at 30

Before:

```text
30
/
20
```

After:

```text
20
 \
 30
```

Entire tree:

```text
10
 \
 20
   \
   30
```

---

# Step 2

Left Rotate at 10

Result:

```text
      20
     /  \
   10   30
```

Balanced.

---

# RL Rule

When:

```text
BF(Z)=-2
```

and

```text
BF(Y)=+1
```

Perform:

```text
Right Rotate(Y)
Left Rotate(Z)
```

---

# 10. Rotation Summary Table

| Case | Pattern     | Rotation       |
| ---- | ----------- | -------------- |
| LL   | Left Left   | Right Rotation |
| RR   | Right Right | Left Rotation  |
| LR   | Left Right  | Left + Right   |
| RL   | Right Left  | Right + Left   |

This table is asked extremely frequently in interviews.

---

# 11. Why Rotations Are O(1)

Consider LL rotation.

Before:

```text
      30
     /
   20
   /
 10
```

Only a few pointers change:

```text
30.left
20.right
```

No traversal required.

Number of pointer updates is constant.

Therefore:

```math
O(1)
```

---

# 12. Why Rotations Preserve BST Property

Suppose:

```text
A < B < C
```

Before rotation:

```text
      C
     /
    B
   /
  A
```

After rotation:

```text
      B
     / \
    A   C
```

Ordering remains:

```text
A < B < C
```

BST property remains intact.

This is why rotations are safe.

---

# 13. Interview Trick

Interviewers often draw:

```text
      40
     /
   20
     \
      30
```

and ask:

> Which rotation is needed?

Method:

Step 1:

Find path from unbalanced node.

```text
40 → 20 → 30
```

Direction:

```text
Left → Right
```

Answer:

```text
LR Rotation
```

Always follow the insertion path.

---

# Common Mistakes

---

## Mistake 1

Memorizing rotations without understanding paths.

Always identify:

```text
LL
RR
LR
RL
```

using insertion direction.

---

## Mistake 2

Using wrong rotation order.

LR:

```text
Left
then
Right
```

RL:

```text
Right
then
Left
```

---

## Mistake 3

Forgetting height updates after rotation.

Many AVL implementations fail because heights are not updated.

---

# Trainer Notes

When teaching students:

1. Draw imbalance physically.
2. Teach LL and RR first.
3. Show why single rotations fail for LR and RL.
4. Then introduce double rotations.
5. Use insertion sequences repeatedly.

A student who can identify rotations instantly usually understands AVL deeply.

---

# Lesson Summary

* AVL imbalance occurs when BF becomes ±2.
* Rotations restore balance.
* Rotations preserve BST ordering.
* Rotations take O(1) time.
* Four cases exist:

| Case | Rotation     |
| ---- | ------------ |
| LL   | Right        |
| RR   | Left         |
| LR   | Left + Right |
| RL   | Right + Left |

* Every AVL insertion can be fixed using these four cases.

---

# Conceptual Questions

1. Why are rotations needed?
2. Why does LL require a right rotation?
3. Why does RR require a left rotation?
4. Why can't LR be fixed using a single rotation?
5. Why can't RL be fixed using a single rotation?
6. Why do rotations preserve BST properties?
7. Why are rotations O(1)?

---

# Coding Exercises

1. Implement Right Rotation.
2. Implement Left Rotation.
3. Detect LL, RR, LR, RL cases.
4. Simulate rotations manually.
5. Write a function that prints which rotation is required for a given insertion sequence.
