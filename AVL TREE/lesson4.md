# Advanced Data Structures & Algorithms

# Module 4: AVL Tree Insertion Algorithm

# Lesson 4: Complete AVL Insertion

---

# Learning Objectives

By the end of this lesson, you will understand:

* How AVL insertion works internally
* Why insertion may cause imbalance
* How balance factors are updated
* How AVL detects LL, RR, LR, RL cases
* Complete recursive AVL insertion algorithm
* Step-by-step dry runs
* Time complexity analysis
* Interview-level implementation details

---

# 1. Recap

So far we have learned:

### Lesson 1

Why AVL Tree was invented.

### Lesson 2

Why AVL height is always:

```math
O(\log n)
```

### Lesson 3

The four rotations:

| Case | Rotation       |
| ---- | -------------- |
| LL   | Right Rotation |
| RR   | Left Rotation  |
| LR   | Left + Right   |
| RL   | Right + Left   |

Now we will combine everything.

---

# 2. The Main Challenge

In a BST, insertion is simple.

Example:

Insert:

```text
50
30
70
20
```

Result:

```text
        50
       /  \
     30    70
    /
   20
```

Insertion ends here.

---

But AVL has an additional responsibility.

After insertion:

```text
Tree must remain balanced.
```

Therefore AVL insertion consists of:

```text
Step 1: Perform BST insertion

Step 2: Update heights

Step 3: Calculate balance factors

Step 4: Detect imbalance

Step 5: Perform rotation
```

---

# 3. High-Level AVL Insertion Algorithm

Whenever we insert a new node:

```text
Insert Node

↓

Update Heights

↓

Compute Balance Factor

↓

Is BF valid?

↓

Yes → Done

No → Rotate
```

This is the complete idea.

---

# 4. AVL Node Structure

A normal BST node contains:

```cpp
data
left
right
```

AVL additionally stores:

```cpp
height
```

Node structure:

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
        left = NULL;
        right = NULL;
        height = 1;
    }
};
```

---

# Why Store Height?

Without height:

To compute Balance Factor:

```cpp
BF = height(left) - height(right)
```

we would have to traverse subtrees repeatedly.

That would increase complexity.

Therefore AVL stores height in every node.

---

# 5. Height Function

---

## Definition

Height of a node:

```text
Longest path from node to leaf
```

---

Example:

```text
      30
     /
   20
   /
 10
```

Heights:

```text
10 → 1

20 → 2

30 → 3
```

---

Function:

```cpp
int getHeight(Node* root)
{
    if(root == NULL)
        return 0;

    return root->height;
}
```

---

Complexity

```math
O(1)
```

because height is already stored.

---

# 6. Balance Factor Function

AVL Balance Factor:

```math
BF = Height(Left) - Height(Right)
```

Implementation:

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

Example

```text
      30
     /
   20
```

Height(left)=1

Height(right)=0

```math
BF=1
```

Balanced.

---

# 7. Right Rotation Implementation

Recall LL Case.

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

---

Pointer Movement

Before:

```text
        y
       /
      x
       \
        T2
```

After:

```text
        x
         \
          y
```

---

Implementation

```cpp
Node* rightRotate(Node* y)
{
    Node* x = y->left;

    Node* T2 = x->right;

    x->right = y;

    y->left = T2;

    y->height =
        max(getHeight(y->left),
            getHeight(y->right))
            + 1;

    x->height =
        max(getHeight(x->left),
            getHeight(x->right))
            + 1;

    return x;
}
```

---

# Understanding Every Line

---

## Step 1

```cpp
Node* x = y->left;
```

Store left child.

---

## Step 2

```cpp
Node* T2 = x->right;
```

Store temporary subtree.

---

## Step 3

```cpp
x->right = y;
```

Rotation occurs.

---

## Step 4

```cpp
y->left = T2;
```

Reconnect subtree.

---

## Step 5

Update heights.

---

# 8. Left Rotation Implementation

RR case.

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

---

Implementation

```cpp
Node* leftRotate(Node* x)
{
    Node* y = x->right;

    Node* T2 = y->left;

    y->left = x;

    x->right = T2;

    x->height =
        max(getHeight(x->left),
            getHeight(x->right))
            + 1;

    y->height =
        max(getHeight(y->left),
            getHeight(y->right))
            + 1;

    return y;
}
```

---

# 9. Complete AVL Insertion

Now the real algorithm.

---

## Step 1

Normal BST insertion.

---

## Step 2

Update height.

---

## Step 3

Compute Balance Factor.

---

## Step 4

Check all four cases.

---

Implementation:

```cpp
Node* insert(Node* root,int key)
{
    // BST INSERTION

    if(root==NULL)
        return new Node(key);

    if(key < root->key)
        root->left =
            insert(root->left,key);

    else if(key > root->key)
        root->right =
            insert(root->right,key);

    else
        return root;

    // UPDATE HEIGHT

    root->height =
        1 +
        max(getHeight(root->left),
            getHeight(root->right));

    // CALCULATE BF

    int balance =
        getBalance(root);

    // LL

    if(balance > 1 &&
       key < root->left->key)
    {
        return rightRotate(root);
    }

    // RR

    if(balance < -1 &&
       key > root->right->key)
    {
        return leftRotate(root);
    }

    // LR

    if(balance > 1 &&
       key > root->left->key)
    {
        root->left =
            leftRotate(root->left);

        return rightRotate(root);
    }

    // RL

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

# 10. Dry Run (LL Case)

Insert:

```text
30
20
10
```

---

After inserting 30

```text
30
```

Balanced.

---

After inserting 20

```text
30
/
20
```

BF=1

Balanced.

---

After inserting 10

```text
30
/
20
/
10
```

BF(30)=2

LL detected.

Right Rotation.

Result:

```text
    20
   /  \
 10   30
```

Balanced.

---

# 11. Dry Run (RR Case)

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

RR.

Apply Left Rotation.

Result:

```text
      20
     /  \
   10   30
```

---

# 12. Dry Run (LR Case)

Insert:

```text
30
10
20
```

Before balancing:

```text
      30
     /
   10
     \
      20
```

---

Step 1

Left Rotate(10)

```text
      30
     /
   20
  /
10
```

---

Step 2

Right Rotate(30)

```text
      20
     /  \
   10   30
```

---

# 13. Dry Run (RL Case)

Insert:

```text
10
30
20
```

Before balancing:

```text
10
 \
 30
 /
20
```

---

Step 1

Right Rotate(30)

```text
10
 \
 20
   \
   30
```

---

Step 2

Left Rotate(10)

```text
      20
     /  \
   10   30
```

---

# 14. Why AVL Insertion is O(log n)

---

## BST Search

We first find insertion position.

AVL height:

```math
O(\log n)
```

Thus:

```math
O(\log n)
```

---

## Height Updates

Only ancestors are updated.

Maximum ancestors:

```math
O(\log n)
```

---

## Rotation

Single rotation:

```math
O(1)
```

Double rotation:

```math
O(1)
```

Still constant.

---

Therefore:

```math
Insertion = O(\log n)
```

---

# 15. Common Interview Questions

---

## Q1

Why store height instead of recalculating?

### Answer

Recalculation would require subtree traversal repeatedly and increase complexity.

---

## Q2

Can insertion require multiple rotations?

### Answer

No.

One balancing operation at the first unbalanced ancestor is sufficient.

---

## Q3

Which node is rotated?

### Answer

The first unbalanced ancestor while moving upward.

---

## Q4

How many nodes are affected by a rotation?

### Answer

Constant number of nodes.

Therefore:

```math
O(1)
```

---

# Common Mistakes

---

## Mistake 1

Updating height before recursive insertion completes.

---

## Mistake 2

Forgetting to return new subtree root after rotation.

---

## Mistake 3

Checking balance before height update.

Wrong order.

---

## Mistake 4

Mixing LL and LR conditions.

Always identify insertion path.

---

# Trainer Notes

To teach AVL insertion:

1. Teach BST insertion first.
2. Show imbalance generation.
3. Update heights manually.
4. Calculate BF manually.
5. Detect LL/RR/LR/RL.
6. Perform rotations by hand.
7. Then show code.

Students understand AVL code much faster after manual simulations.

---

# Lesson Summary

AVL insertion:

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

Complexity:

| Operation | Complexity |
| --------- | ---------- |
| Search    | O(log n)   |
| Insert    | O(log n)   |
| Rotation  | O(1)       |

AVL insertion guarantees logarithmic height and efficient operations.

---

# Conceptual Questions

1. Why do we update height after insertion?
2. Why is Balance Factor needed?
3. Why is rotation O(1)?
4. Why does AVL insertion remain O(log n)?
5. Why is only the first unbalanced ancestor fixed?
6. Why do we return the new root after rotation?

---

# Coding Exercises

1. Implement AVL insertion.
2. Print Balance Factor of every node.
3. Count total rotations performed.
4. Visualize tree after every insertion.
5. Build AVL Tree from an array and verify balance.
