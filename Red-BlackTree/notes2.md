# Red-Black Tree Notes

---

# 1. Introduction

## What is a Red-Black Tree?

A Red-Black Tree (RBT) is a self-balancing Binary Search Tree (BST) that maintains balance using node colors.

Each node stores:

```cpp
RED
or
BLACK
```

in addition to the key.

---

## Why is it Needed?

Normal BST can become:

```text
1
 \
  2
   \
    3
     \
      4
```

Height:

```cpp
O(n)
```

Search:

```cpp
O(n)
```

Red-Black Trees maintain:

```cpp
Height = O(log n)
```

thus guaranteeing:

```cpp
Search = O(log n)
Insert = O(log n)
Delete = O(log n)
```

---

# 2. Core Idea

AVL Tree maintains balance using:

```cpp
Height
Balance Factor
```

Red-Black Tree maintains balance using:

```cpp
Colors
```

---

# 3. Red-Black Properties

Every valid Red-Black Tree satisfies:

### Property 1

Every node is:

```cpp
RED
or
BLACK
```

---

### Property 2

Root is BLACK.

---

### Property 3

Every NIL leaf is BLACK.

---

### Property 4

No RED node can have a RED child.

Valid:

```text
B
 \
  R
```

Invalid:

```text
B
 \
  R
   \
    R
```

---

### Property 5

Every path from a node to descendant NIL nodes contains the same number of BLACK nodes.

This count is called:

```cpp
Black Height
```

---

# 4. Black Height

Definition:

```cpp
Black Height =
Number of BLACK nodes
from node to NIL
```

Notation:

```cpp
bh(x)
```

---

## Why Important?

It guarantees balance.

Every path contains equal black nodes.

---

# 5. Why New Nodes Are Inserted RED?

Interview Favorite

If inserted BLACK:

```cpp
Black Height changes
```

Property 5 may break.

If inserted RED:

```cpp
Black Height unchanged
```

Therefore:

```cpp
New Node = RED
```

---

# 6. Insertion Cases

Insertion may create:

```cpp
RED Parent
RED Child
```

violation.

---

## Family Terms

```text
        G
       / \
      P   U
     /
    X
```

```cpp
G = Grandparent
P = Parent
U = Uncle
X = Inserted Node
```

---

## Case 1: Uncle RED

```text
      B
     / \
    R   R
   /
  R
```

Action:

```cpp
Parent -> BLACK
Uncle -> BLACK
Grandparent -> RED
```

Called:

```cpp
Recoloring
```

---

## Case 2: Uncle BLACK

Use rotations.

---

### LL

```text
G
/
P
/
X
```

Action:

```cpp
Right Rotation
```

---

### RR

```text
G
 \
  P
   \
    X
```

Action:

```cpp
Left Rotation
```

---

### LR

```text
G
/
P
 \
  X
```

Action:

```cpp
Left Rotate(P)
Right Rotate(G)
```

---

### RL

```text
G
 \
  P
 /
X
```

Action:

```cpp
Right Rotate(P)
Left Rotate(G)
```

---

# 7. Deletion

Deletion is harder than insertion.

Reason:

```cpp
Deleting BLACK node
changes black height.
```

---

# Double Black Concept

Missing BLACK node is represented as:

```cpp
DOUBLE BLACK
```

(DB)

---

Example:

```text
     B
    /
   DB
```

---

# Deletion Cases

Deletion is based on:

```cpp
Sibling
```

instead of Uncle.

---

## Case 1

Sibling RED

```cpp
Rotate
Recolor
```

Convert to another case.

---

## Case 2

Sibling BLACK
Children BLACK

```cpp
Push Double Black Upward
```

---

## Case 3

Sibling BLACK
Near Child RED

```cpp
Convert to Case 4
```

---

## Case 4

Sibling BLACK
Far Child RED

```cpp
Rotate
Recolor
Done
```

---

# 8. Height Proof

Property 4:

```cpp
No consecutive RED nodes
```

Therefore:

```cpp
Longest Path
≤
2 × Shortest Path
```

---

Property 5:

```cpp
Equal Black Height
```

guarantees:

```cpp
bh ≤ log₂(n+1)
```

Thus:

```cpp
Height ≤ 2log₂(n+1)
```

Therefore:

```cpp
Height = O(log n)
```

---

# 9. Complexity

| Operation | Complexity |
| --------- | ---------- |
| Search    | O(log n)   |
| Insert    | O(log n)   |
| Delete    | O(log n)   |
| Rotation  | O(1)       |

---

# 10. Node Structure

```cpp
enum Color
{
    RED,
    BLACK
};

struct Node
{
    int data;

    Color color;

    Node* left;
    Node* right;
    Node* parent;

    Node(int val)
    {
        data = val;

        color = RED;

        left = NULL;
        right = NULL;
        parent = NULL;
    }
};
```

---

# 11. Left Rotation

```cpp
void leftRotate(Node*& root, Node*& x)
{
    Node* y = x->right;

    x->right = y->left;

    if(y->left)
        y->left->parent = x;

    y->parent = x->parent;

    if(x->parent == NULL)
        root = y;

    else if(x == x->parent->left)
        x->parent->left = y;

    else
        x->parent->right = y;

    y->left = x;
    x->parent = y;
}
```

---

# 12. Right Rotation

```cpp
void rightRotate(Node*& root, Node*& y)
{
    Node* x = y->left;

    y->left = x->right;

    if(x->right)
        x->right->parent = y;

    x->parent = y->parent;

    if(y->parent == NULL)
        root = x;

    else if(y == y->parent->left)
        y->parent->left = x;

    else
        y->parent->right = x;

    x->right = y;
    y->parent = x;
}
```

---

# 13. Insertion Algorithm

```text
BST Insert

↓

Color New Node RED

↓

Parent BLACK?
Done

↓

Parent RED

↓

Check Uncle

↓

RED Uncle
Recolor

↓

BLACK Uncle
Rotate

↓

Root BLACK
```

---

# 14. Deletion Algorithm

```text
BST Delete

↓

Deleted RED?
Done

↓

Deleted BLACK

↓

Create Double Black

↓

Check Sibling

↓

Case 1
Case 2
Case 3
Case 4

↓

Remove Double Black
```

---

# 15. AVL vs Red-Black

| Feature   | AVL     | Red-Black       |
| --------- | ------- | --------------- |
| Balance   | Strict  | Relaxed         |
| Height    | Smaller | Larger          |
| Search    | Faster  | Slightly Slower |
| Insert    | Slower  | Faster          |
| Delete    | Slower  | Faster          |
| Rotations | More    | Fewer           |
| STL Usage | No      | Yes             |

---

# 16. BST vs AVL vs Red-Black

| Feature | BST  | AVL      | RBT      |
| ------- | ---- | -------- | -------- |
| Search  | O(n) | O(log n) | O(log n) |
| Insert  | O(n) | O(log n) | O(log n) |
| Delete  | O(n) | O(log n) | O(log n) |
| Balance | No   | Strict   | Relaxed  |

---

# 17. STL Containers Using RBT

Internally:

```cpp
map
set
multimap
multiset
```

use:

```cpp
Red-Black Tree
```

---

# 18. Conceptual Questions

1. Why are Red-Black Trees needed if AVL already exists?
2. Why are new nodes inserted RED?
3. Why are NIL nodes BLACK?
4. What is Black Height?
5. Why are consecutive RED nodes forbidden?
6. Why does insertion use Uncle?
7. Why does deletion use Sibling?
8. What is Double Black?
9. Why does STL use Red-Black Trees?
10. Why is height ≤ 2log₂(n+1)?

---

# 19. Coding Questions

## Easy

1. Implement Left Rotation.
2. Implement Right Rotation.
3. Calculate Black Height.
4. Validate Red-Black Properties.

---

## Medium

1. Implement Red-Black Insertion.
2. Implement Recoloring.
3. Detect LL/RR/LR/RL cases.
4. Convert BST to Red-Black Tree.

---

## Hard

1. Implement Complete Red-Black Tree.
2. Implement FixInsert().
3. Implement FixDelete().
4. Implement Order Statistics Tree using RBT.
5. Design STL-like map using Red-Black Tree.

---

# 20. Most Asked Interview Questions

### Why insert RED?

Because RED insertion does not change black height.

---

### Why root must be BLACK?

Property 2.

Simplifies balancing.

---

### Why is deletion harder?

Deleting BLACK nodes affects black height.

---

### What is Double Black?

A conceptual extra blackness representing a missing BLACK node.

---

### Why does STL use RBT?

Fewer rotations during updates.

---

### AVL or Red-Black for searching?

AVL.

---

### AVL or Red-Black for updates?

Red-Black.

---

# 21. 60-Second Revision Sheet

```text
Red-Black Tree = Self Balancing BST

Properties:
1. Every node RED/BLACK
2. Root BLACK
3. NIL BLACK
4. No consecutive RED
5. Equal Black Height

Insert:
RED Node
RED Uncle -> Recolor
BLACK Uncle -> Rotate

Delete:
Double Black
Sibling Cases

Height <= 2log₂(n+1)

Search = O(log n)
Insert = O(log n)
Delete = O(log n)

STL:
map
set
multimap
multiset

AVL:
Better Search

RBT:
Better Updates
```
