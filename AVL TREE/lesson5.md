# Advanced Data Structures & Algorithms

# Module 5: AVL Tree Deletion

# Lesson 5: Complete AVL Deletion Algorithm

---

# Learning Objectives

By the end of this lesson, you will understand:

* Why AVL deletion is harder than insertion
* BST deletion review
* All deletion cases
* Rebalancing after deletion
* Why multiple nodes can become unbalanced
* Complete AVL delete algorithm
* Dry runs
* Complexity analysis
* Interview-level edge cases

---

# 1. Why AVL Deletion is Harder Than Insertion

Many students think:

> If AVL insertion is easy, deletion must also be easy.

This is not true.

In fact:

```text
AVL Deletion > AVL Insertion
```

in terms of difficulty.

---

# Why?

During insertion:

* Height increases on only one path.
* Only one ancestor usually becomes unbalanced.
* One balancing operation is enough.

During deletion:

* Height can decrease.
* Multiple ancestors may become unbalanced.
* Rebalancing may propagate upward.

This makes deletion more complex.

---

# 2. Recap: BST Deletion

AVL deletion begins exactly like BST deletion.

Therefore we must first understand BST deletion.

---

# Case 1: Leaf Node Deletion

Example:

```text
      50
     /  \
   30    70
```

Delete:

```text
30
```

Result:

```text
      50
        \
         70
```

Very simple.

---

# Case 2: One Child

Example:

```text
      50
     /
   30
   /
 20
```

Delete:

```text
30
```

Result:

```text
      50
     /
   20
```

Child replaces parent.

---

# Case 3: Two Children

Example:

```text
        50
       /  \
     30    70
          /  \
        60    80
```

Delete:

```text
70
```

---

## Step 1

Find inorder successor.

```text
80
```

or

Find inorder predecessor.

```text
60
```

Most implementations use successor.

---

## Step 2

Replace node value.

```text
70 → 80
```

---

## Step 3

Delete actual successor node.

Result:

```text
        50
       /  \
     30    80
          /
        60
```

BST property remains valid.

---

# 3. AVL Deletion Overview

AVL deletion consists of:

```text
Delete Node

↓

Perform BST Deletion

↓

Update Heights

↓

Calculate Balance Factors

↓

Detect Imbalance

↓

Perform Rotations

↓

Continue Moving Upward
```

The last step is what makes deletion harder.

---

# 4. Example of Height Reduction

Consider:

```text
          40
         /  \
       20    60
      / \
    10  30
```

Delete:

```text
30
```

Tree:

```text
          40
         /  \
       20    60
      /
    10
```

Height of subtree rooted at 20 decreases.

This change may affect 40.

Now ancestors must be rechecked.

---

# 5. Important Difference

Insertion:

```text
Height may increase.
```

Deletion:

```text
Height may decrease.
```

When height decreases:

Balance factors change differently.

Many interview mistakes happen here.

---

# 6. Rebalancing After Deletion

Exactly the same four rotations are used.

| Imbalance | Rotation       |
| --------- | -------------- |
| LL        | Right Rotation |
| RR        | Left Rotation  |
| LR        | Left + Right   |
| RL        | Right + Left   |

The rotations themselves do not change.

Only detection logic changes slightly.

---

# 7. AVL Delete Algorithm

---

## Step 1

Perform standard BST deletion.

---

## Step 2

If tree becomes empty:

```cpp
if(root==NULL)
    return root;
```

---

## Step 3

Update height.

```cpp
root->height =
1 +
max(getHeight(root->left),
    getHeight(root->right));
```

---

## Step 4

Compute balance factor.

```cpp
int balance =
getBalance(root);
```

---

## Step 5

Check all four cases.

---

# 8. LL Case After Deletion

Condition:

```cpp
balance > 1 &&
getBalance(root->left) >= 0
```

Rotation:

```cpp
return rightRotate(root);
```

---

# Why >= 0 ?

Insertion used:

```cpp
> 0
```

Deletion uses:

```cpp
>= 0
```

because subtree height may have decreased.

This is a famous interview question.

---

# Example

Before:

```text
          40
         /
       20
      /
    10
```

Balance:

```text
BF(40)=+2
```

Perform:

```text
Right Rotation
```

Result:

```text
       20
      /  \
    10    40
```

---

# 9. RR Case After Deletion

Condition:

```cpp
balance < -1 &&
getBalance(root->right) <= 0
```

Rotation:

```cpp
return leftRotate(root);
```

---

# Example

```text
10
 \
 20
   \
   30
```

RR imbalance.

Apply:

```text
Left Rotation
```

Result:

```text
      20
     /  \
   10   30
```

---

# 10. LR Case After Deletion

Condition:

```cpp
balance > 1 &&
getBalance(root->left) < 0
```

Operations:

```cpp
root->left =
leftRotate(root->left);

return rightRotate(root);
```

---

# Example

```text
        40
       /
     20
       \
        30
```

Path:

```text
Left → Right
```

LR imbalance.

Result:

```text
       30
      /  \
    20    40
```

---

# 11. RL Case After Deletion

Condition:

```cpp
balance < -1 &&
getBalance(root->right) > 0
```

Operations:

```cpp
root->right =
rightRotate(root->right);

return leftRotate(root);
```

---

# Example

```text
10
 \
 40
 /
30
```

Path:

```text
Right → Left
```

RL imbalance.

Result:

```text
       30
      /  \
    10    40
```

---

# 12. Finding Minimum Node

Needed during two-child deletion.

---

Example:

```text
         70
        /
      60
     /
   55
```

Minimum:

```text
55
```

Observation:

Minimum is always the leftmost node.

---

Implementation:

```cpp
Node* minValueNode(Node* node)
{
    Node* current = node;

    while(current->left != NULL)
    {
        current = current->left;
    }

    return current;
}
```

---

# Complexity

Height of AVL:

```math
O(\log n)
```

Traversal:

```math
O(\log n)
```

---

# 13. Complete AVL Delete Implementation

```cpp
Node* deleteNode(Node* root,int key)
{
    if(root==NULL)
        return root;

    if(key < root->key)
    {
        root->left =
        deleteNode(root->left,key);
    }

    else if(key > root->key)
    {
        root->right =
        deleteNode(root->right,key);
    }

    else
    {
        // Node found

        if(root->left==NULL ||
           root->right==NULL)
        {
            Node* temp =
            root->left ?
            root->left :
            root->right;

            if(temp==NULL)
            {
                temp=root;
                root=NULL;
            }
            else
            {
                *root=*temp;
            }

            delete temp;
        }
        else
        {
            Node* temp =
            minValueNode(root->right);

            root->key=temp->key;

            root->right =
            deleteNode(root->right,
                       temp->key);
        }
    }

    if(root==NULL)
        return root;

    root->height =
    1 +
    max(getHeight(root->left),
        getHeight(root->right));

    int balance =
    getBalance(root);

    // LL

    if(balance>1 &&
       getBalance(root->left)>=0)
    {
        return rightRotate(root);
    }

    // LR

    if(balance>1 &&
       getBalance(root->left)<0)
    {
        root->left =
        leftRotate(root->left);

        return rightRotate(root);
    }

    // RR

    if(balance<-1 &&
       getBalance(root->right)<=0)
    {
        return leftRotate(root);
    }

    // RL

    if(balance<-1 &&
       getBalance(root->right)>0)
    {
        root->right =
        rightRotate(root->right);

        return leftRotate(root);
    }

    return root;
}
```

---

# 14. Dry Run

Initial Tree:

```text
          30
         /  \
       20    40
      / \
    10  25
```

Delete:

```text
40
```

Result:

```text
          30
         /
       20
      / \
    10  25
```

---

Balance Factor:

```text
BF(30)=2
```

Unbalanced.

---

Left Child BF:

```text
BF(20)=0
```

Condition:

```text
LL
```

Apply:

```text
Right Rotation
```

Result:

```text
         20
        /  \
      10    30
           /
         25
```

Balanced.

---

# 15. Why AVL Deletion is O(log n)

---

## Search Phase

Finding node:

```math
O(\log n)
```

---

## Height Updates

Only ancestors affected.

Maximum:

```math
O(\log n)
```

---

## Rotations

Each rotation:

```math
O(1)
```

---

Total:

```math
O(\log n)
```

---

# AVL Complexity Table

| Operation | Complexity |
| --------- | ---------- |
| Search    | O(log n)   |
| Insert    | O(log n)   |
| Delete    | O(log n)   |
| Rotation  | O(1)       |

---

# Common Interview Questions

---

## Q1

Why is AVL deletion harder than insertion?

### Answer

Deletion can reduce subtree height and may affect multiple ancestors.

---

## Q2

Why do we use inorder successor?

### Answer

It preserves BST ordering.

---

## Q3

Can deletion trigger multiple rebalancing operations?

### Answer

Yes.

Unlike insertion, deletion may propagate imbalance upward.

---

## Q4

Why does LL condition use >=0 in deletion?

### Answer

Because height reduction can create a balance factor of exactly 0.

---

# Common Mistakes

---

## Mistake 1

Forgetting to update height after deletion.

---

## Mistake 2

Using insertion conditions during deletion.

---

## Mistake 3

Deleting successor incorrectly.

---

## Mistake 4

Not checking:

```cpp
if(root==NULL)
```

after deletion.

---

# Trainer Notes

When teaching deletion:

1. Revise BST deletion first.
2. Explain leaf, one-child, two-child cases.
3. Show height reduction visually.
4. Demonstrate imbalance propagation.
5. Then introduce AVL rebalancing.

Most students struggle because they try to learn AVL deletion without fully understanding BST deletion.

---

# Lesson Summary

AVL deletion:

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

Key insight:

> Deletion is harder because subtree height decreases and imbalance may propagate upward.

---

# Conceptual Questions

1. Why is AVL deletion harder than insertion?
2. Why do we use inorder successor?
3. Why can deletion affect multiple ancestors?
4. Why is AVL deletion still O(log n)?
5. Why do LL and RR conditions differ slightly from insertion?

---

# Coding Exercises

1. Implement AVL deletion from scratch.
2. Delete leaf nodes repeatedly.
3. Delete nodes with one child.
4. Delete nodes with two children.
5. Print balance factors after every deletion.
6. Count total rotations performed during deletions.
