# AVL Tree Complete Notes

## Table of Contents

1. Introduction
2. Why AVL Tree?
3. Balance Factor
4. AVL Properties
5. Height Analysis
6. AVL Rotations
7. Rotation Decision Table
8. AVL Node Structure
9. Helper Functions
10. AVL Insertion
11. AVL Deletion
12. Complexity Analysis
13. AVL vs BST
14. AVL vs Red-Black Tree
15. Interview Questions
16. Common Mistakes
17. Technical Trainer Notes
18. Final Revision Sheet

---

# 1. Introduction

AVL Tree is a self-balancing Binary Search Tree (BST).

It was invented by:

* Adelson-Velsky
* Landis

Hence the name AVL.

Main objective:

> Maintain BST height close to logarithmic after insertions and deletions.

---

# 2. Why AVL Tree?

## Problem with BST

Insert:

```text
10 20 30 40 50
```

BST becomes:

```text
10
 \
 20
   \
   30
     \
      40
        \
         50
```

Height:

```text
O(n)
```

Search:

```text
O(n)
```

BST behaves like a linked list.

---

## AVL Solution

AVL automatically rebalances the tree.

Result:

```text
      30
     /  \
   20    40
  /        \
10          50
```

Height:

```text
O(log n)
```

---

# 3. Balance Factor

Definition:

```cpp
BF = Height(Left) - Height(Right)
```

---

## Example

```text
      30
     /  \
   20   40
```

```cpp
BF = 1 - 1 = 0
```

Balanced.

---

## Example

```text
      30
     /
   20
```

```cpp
BF = 1 - 0 = 1
```

Balanced.

---

## Example

```text
30
/
20
/
10
```

```cpp
BF = 2 - 0 = 2
```

Unbalanced.

---

# 4. AVL Properties

For every node:

```cpp
-1 <= BF <= 1
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

# 5. Height Analysis

AVL maintains logarithmic height.

Minimum node recurrence:

```cpp
N(h) = 1 + N(h-1) + N(h-2)
```

This follows Fibonacci growth.

Therefore:

```cpp
N(h) ≈ φ^h
```

Taking logarithm:

```cpp
h = O(log n)
```

Exact AVL bound:

```cpp
h <= 1.44 * log₂(n+2)
```

---

# 6. AVL Rotations

There are four rotations:

1. LL
2. RR
3. LR
4. RL

---

## LL Rotation

Condition:

```cpp
BF(Node) = +2
BF(Left Child) >= 0
```

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

Rotation:

```cpp
Right Rotate
```

---

## RR Rotation

Condition:

```cpp
BF(Node) = -2
BF(Right Child) <= 0
```

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

Rotation:

```cpp
Left Rotate
```

---

## LR Rotation

Condition:

```cpp
BF(Node) = +2
BF(Left Child) < 0
```

Before:

```text
      30
     /
   10
     \
      20
```

Operations:

```cpp
Left Rotate(10)
Right Rotate(30)
```

After:

```text
      20
     /  \
   10   30
```

---

## RL Rotation

Condition:

```cpp
BF(Node) = -2
BF(Right Child) > 0
```

Before:

```text
10
 \
 30
 /
20
```

Operations:

```cpp
Right Rotate(30)
Left Rotate(10)
```

After:

```text
      20
     /  \
   10   30
```

---

# 7. Rotation Decision Table ⭐

| BF(Node) | BF(Child) | Case | Rotation     |
| -------- | --------- | ---- | ------------ |
| +2       | >= 0      | LL   | Right Rotate |
| +2       | < 0       | LR   | Left + Right |
| -2       | <= 0      | RR   | Left Rotate  |
| -2       | > 0       | RL   | Right + Left |

---

## Fast Interview Trick

```text
+2 => Look Left Child

-2 => Look Right Child
```

Then:

```text
Left + Left   => LL

Left + Right  => LR

Right + Right => RR

Right + Left  => RL
```

---

# 8. AVL Node Structure

```cpp
struct Node
{
    int key;
    Node* left;
    Node* right;
    int height;

    Node(int val)
    {
        key = val;
        left = right = NULL;
        height = 1;
    }
};
```

---

# 9. Helper Functions

## Height Function

```cpp
int getHeight(Node* root)
{
    if(root == NULL)
        return 0;

    return root->height;
}
```

---

## Balance Factor

```cpp
int getBalance(Node* root)
{
    if(root == NULL)
        return 0;

    return getHeight(root->left)
         - getHeight(root->right);
}
```

---

# 10. Rotation Code

## Right Rotation

```cpp
Node* rightRotate(Node* y)
{
    Node* x = y->left;
    Node* T2 = x->right;

    x->right = y;
    y->left = T2;

    y->height =
        1 + max(getHeight(y->left),
                getHeight(y->right));

    x->height =
        1 + max(getHeight(x->left),
                getHeight(x->right));

    return x;
}
```

Complexity:

```cpp
O(1)
```

---

## Left Rotation

```cpp
Node* leftRotate(Node* x)
{
    Node* y = x->right;
    Node* T2 = y->left;

    y->left = x;
    x->right = T2;

    x->height =
        1 + max(getHeight(x->left),
                getHeight(x->right));

    y->height =
        1 + max(getHeight(y->left),
                getHeight(y->right));

    return y;
}
```

Complexity:

```cpp
O(1)
```

---

# 11. AVL Insertion

Steps:

```text
BST Insert

↓

Update Height

↓

Calculate BF

↓

Detect Case

↓

Rotate
```

---

## Insertion Conditions

LL

```cpp
if(balance > 1 &&
   key < root->left->key)
```

RR

```cpp
if(balance < -1 &&
   key > root->right->key)
```

LR

```cpp
if(balance > 1 &&
   key > root->left->key)
```

RL

```cpp
if(balance < -1 &&
   key < root->right->key)
```

---

## Complete AVL Insertion Code

```cpp
Node* insert(Node* root,int key)
{
    if(root==NULL)
        return new Node(key);

    if(key < root->key)
        root->left = insert(root->left,key);

    else if(key > root->key)
        root->right = insert(root->right,key);

    else
        return root;

    root->height =
        1 +
        max(getHeight(root->left),
            getHeight(root->right));

    int balance = getBalance(root);

    if(balance > 1 &&
       key < root->left->key)
        return rightRotate(root);

    if(balance < -1 &&
       key > root->right->key)
        return leftRotate(root);

    if(balance > 1 &&
       key > root->left->key)
    {
        root->left =
            leftRotate(root->left);

        return rightRotate(root);
    }

    if(balance < -1 &&
       key < root->right->key)
    {
        root->right =
            rightRotate(root->right);

        return leftRotate(root);
    }

    return root;
}
```

---

# 12. AVL Deletion

Steps:

```text
BST Delete

↓

Update Height

↓

Calculate BF

↓

Detect Case

↓

Rotate

↓

Continue Upward
```

---

## Minimum Value Node

```cpp
Node* minValueNode(Node* node)
{
    Node* current = node;

    while(current->left != NULL)
        current = current->left;

    return current;
}
```

---

## Deletion Conditions

LL

```cpp
balance > 1 &&
getBalance(root->left) >= 0
```

RR

```cpp
balance < -1 &&
getBalance(root->right) <= 0
```

LR

```cpp
balance > 1 &&
getBalance(root->left) < 0
```

RL

```cpp
balance < -1 &&
getBalance(root->right) > 0
```

---

## Complete AVL Deletion Code

(Use the standard AVL delete implementation discussed earlier.)

Key idea:

```text
Delete

↓

Update Height

↓

Calculate BF

↓

Rotate

↓

Continue Upward
```

---

# 13. Complexity Analysis

| Operation     | Complexity |
| ------------- | ---------- |
| Search        | O(log n)   |
| Insert        | O(log n)   |
| Delete        | O(log n)   |
| Rotation      | O(1)       |
| Height Access | O(1)       |

---

# Why?

AVL height:

```cpp
O(log n)
```

All operations follow a single root-to-leaf path.

---

# 14. AVL vs BST

| Feature  | BST  | AVL      |
| -------- | ---- | -------- |
| Balanced | No   | Yes      |
| Height   | O(n) | O(log n) |
| Search   | O(n) | O(log n) |
| Insert   | O(n) | O(log n) |
| Delete   | O(n) | O(log n) |

---

# 15. AVL vs Red-Black Tree

| Feature   | AVL     | Red-Black       |
| --------- | ------- | --------------- |
| Balance   | Strict  | Relaxed         |
| Search    | Faster  | Slightly Slower |
| Insert    | Slower  | Faster          |
| Delete    | Slower  | Faster          |
| Rotations | More    | Fewer           |
| Height    | Smaller | Larger          |

---

# 16. Frequently Asked Interview Questions

## What is AVL Tree?

Self-balancing BST.

---

## What is Balance Factor?

```cpp
Height(Left)-Height(Right)
```

---

## Why AVL?

To avoid skewed BST.

---

## Why store height?

To calculate BF in O(1).

---

## Why are rotations O(1)?

Only constant number of pointers change.

---

## How many rotations?

```text
LL
RR
LR
RL
```

---

## Which rotation for LL?

```text
Right Rotation
```

---

## Which rotation for RR?

```text
Left Rotation
```

---

## Why AVL Search O(log n)?

Because AVL height is O(log n).

---

## Which STL container uses AVL?

None.

```cpp
map
set
multiset
multimap
```

use Red-Black Trees.

---

# 17. Common Mistakes

### Mistake 1

Forgetting to update height.

---

### Mistake 2

Wrong BF formula.

Correct:

```cpp
Height(Left)-Height(Right)
```

---

### Mistake 3

Mixing LL and LR.

---

### Mistake 4

Mixing RR and RL.

---

### Mistake 5

Forgetting to return new root after rotation.

---

# 18. Technical Trainer Notes

Teaching Order:

1. BST
2. Skewed BST Problem
3. Height Concept
4. Balance Factor
5. LL and RR
6. LR and RL
7. Insertion
8. Deletion
9. Complexity Analysis
10. AVL vs Red-Black

---

# Final 60-Second Revision

```text
AVL = Self Balancing BST

BF = Height(Left) - Height(Right)

Allowed BF:
-1,0,+1

BF > 1 => Left Heavy
BF < -1 => Right Heavy

+2 + Left Child >=0 => LL => Right Rotate

+2 + Left Child <0 => LR => Left + Right

-2 + Right Child <=0 => RR => Left Rotate

-2 + Right Child >0 => RL => Right + Left

Search  = O(log n)
Insert  = O(log n)
Delete  = O(log n)

Rotation = O(1)

Height <= 1.44 log₂(n+2)

STL map/set => Red-Black Tree
```

# AVL Mastery Checklist

* [ ] Understand Balance Factor
* [ ] Understand all 4 rotations
* [ ] Implement insertion
* [ ] Implement deletion
* [ ] Explain complexity
* [ ] Compare AVL vs Red-Black
* [ ] Teach AVL to a beginner
* [ ] Solve interview questions confidently
