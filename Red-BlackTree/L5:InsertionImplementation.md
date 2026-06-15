# Red-Black Tree Complete Notes

# Module 5: Complete Red-Black Tree Insertion Algorithm & C++ Implementation

---

# Learning Objectives

By the end of this module, you will understand:

* Complete Red-Black insertion algorithm
* Why insertion has two phases
* The fix-up procedure
* Parent pointers
* Detailed dry runs
* Production-level C++ implementation
* Complexity analysis
* Interview questions

---

# 1. Recap

We learned:

### New nodes are inserted as RED

because:

```text
RED nodes do not affect black height.
```

---

After insertion:

Usually only:

```text
Property 4
(No two consecutive RED nodes)
```

can break.

---

To fix violations we use:

```text
Parent
Grandparent
Uncle
```

and perform:

```text
Recoloring
or
Rotations
```

---

# 2. High-Level Insertion Process

Red-Black insertion consists of:

```text
Step 1:
Normal BST Insertion

↓

Step 2:
Color New Node RED

↓

Step 3:
Fix Violations
```

---

# Why Two Phases?

Because:

A Red-Black Tree is still a BST.

Therefore:

```text
Searching position
```

works exactly like BST.

Balancing comes later.

---

# Example

Insert:

```text
50
30
70
20
```

BST insertion:

```text
        50
       /  \
     30    70
    /
   20
```

Only after insertion do we check Red-Black properties.

---

# 3. Why Parent Pointer Is Necessary

AVL insertion:

```text
Uses recursion.
```

Red-Black insertion:

Needs frequent access to:

```text
Parent
Grandparent
Uncle
```

Therefore nodes store:

```cpp
parent
```

pointer.

---

# Node Structure

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

# 4. Phase 1: BST Insertion

Exactly like BST.

---

Example

Insert:

```text
40
20
60
10
```

Result:

```text
         40
        /  \
      20    60
     /
   10
```

---

New node:

```text
10
```

is inserted RED.

---

# Tree Actually Looks Like

```text
         40(B)
        /    \
     20(R)   60(R)
     /
   10(R)
```

Violation:

```text
20(R)
10(R)
```

Consecutive RED nodes.

---

Need fixing.

---

# 5. The Fix-Up Loop

Core idea:

As long as:

```cpp
parent is RED
```

the tree remains invalid.

Therefore:

```cpp
while(parent is RED)
{
    fix violation
}
```

This is the heart of insertion.

---

# 6. Case 1: Parent is Left Child

Suppose:

```text
          G
         /
       P
      /
     X
```

Parent is left child of grandparent.

---

Uncle:

```text
G.right
```

---

Now check uncle.

---

# Case 1A

# Uncle is RED

---

Example:

```text
             B
           /   \
          R     R
         /
        R
```

---

Apply recoloring:

```text
Parent -> BLACK

Uncle -> BLACK

Grandparent -> RED
```

Result:

```text
             R
           /   \
          B     B
         /
        R
```

---

Now move upward.

Set:

```cpp
node = grandparent;
```

Continue.

---

# Why Move Upward?

Because grandparent became RED.

It might now violate property 4 with its own parent.

---

# Case 1B

# Uncle is BLACK

# LL

---

Example:

```text
         G(B)
        /
      P(R)
      /
    X(R)
```

---

Apply:

```text
Right Rotation(G)
```

After rotation:

```text
        P
       / \
      X   G
```

---

Swap colors:

```text
P -> BLACK

G -> RED
```

---

Result:

```text
       P(B)
      /   \
   X(R)  G(R)
```

Valid.

---

# Case 1C

# Uncle is BLACK

# LR

---

Example:

```text
         G(B)
        /
      P(R)
         \
          X(R)
```

---

Step 1

Left Rotate(P)

```text
         G
        /
       X
      /
     P
```

---

Step 2

Right Rotate(G)

```text
         X
        / \
       P   G
```

---

Color Adjustment

```text
X -> BLACK

G -> RED
```

---

# Important Observation

LR becomes LL first.

Then LL is solved.

---

# 7. Mirror Cases

Everything above assumed:

```text
Parent = Left Child
```

---

Now consider:

```text
Parent = Right Child
```

Mirror image.

---

# Case 2A

Uncle RED

Same recoloring.

---

# Case 2B

RR

Apply:

```text
Left Rotation
```

---

# Case 2C

RL

Apply:

```text
Right Rotation

then

Left Rotation
```

---

# Interview Trick

Only learn:

```text
LEFT SIDE CASES
```

Right side cases are exact mirror images.

---

# 8. Visual Summary

---

Parent RED?

```text
No
↓
Done
```

---

Parent RED?

```text
Yes
↓
Check Uncle
```

---

Uncle RED?

```text
Yes
↓
Recolor
```

---

Uncle BLACK?

```text
Yes
↓
Rotate
```

---

Rotation Types

```text
LL -> Right Rotate

RR -> Left Rotate

LR -> Left + Right

RL -> Right + Left
```

---

# 9. Complete Insertion Pseudocode

```cpp
Insert node as BST

Color node RED

while(parent is RED)
{
    if(uncle is RED)
    {
        recolor
    }
    else
    {
        perform rotation
    }
}

root = BLACK
```

---

# Why Root Must Become Black?

Property 2:

```text
Root must be BLACK
```

Always enforce this at the end.

---

# 10. Left Rotation

Same as AVL.

---

Before

```text
     x
      \
       y
```

---

After

```text
      y
     /
    x
```

---

Code

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

# 11. Right Rotation

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

# 12. Fix Insert Function

This is the most important code.

```cpp
void fixInsert(Node*& root, Node*& pt)
{
    Node* parent = NULL;
    Node* grandparent = NULL;

    while(pt != root &&
          pt->color == RED &&
          pt->parent->color == RED)
    {
        parent = pt->parent;

        grandparent = parent->parent;

        if(parent == grandparent->left)
        {
            Node* uncle =
                grandparent->right;

            if(uncle &&
               uncle->color == RED)
            {
                grandparent->color = RED;
                parent->color = BLACK;
                uncle->color = BLACK;

                pt = grandparent;
            }
            else
            {
                // rotations
            }
        }

        else
        {
            // mirror cases
        }
    }

    root->color = BLACK;
}
```

---

# Interview Note

Most companies do NOT ask candidates to write the complete Red-Black implementation from memory.

They usually ask:

1. Explain insertion.
2. Explain recoloring.
3. Explain LL/RR/LR/RL.
4. Explain why root becomes black.

Understanding matters more than memorization.

---

# 13. Dry Run

Insert:

```text
10
20
30
```

---

Insert 10

```text
10(B)
```

---

Insert 20

```text
10(B)
   \
   20(R)
```

Valid.

---

Insert 30

```text
10(B)
   \
   20(R)
      \
      30(R)
```

Violation.

---

Parent:

```text
20
```

Grandparent:

```text
10
```

Uncle:

```text
NIL(B)
```

RR Case.

Apply:

```text
Left Rotation
```

Result:

```text
      20(B)
     /    \
 10(R)   30(R)
```

Valid.

---

# 14. Complexity Analysis

Search insertion position:

```text
O(log n)
```

---

Recoloring:

```text
O(1)
```

---

Rotation:

```text
O(1)
```

---

Maximum levels visited:

```text
O(log n)
```

---

Total:

```text
Insertion = O(log n)
```

---

# AVL vs Red-Black Insertion

| Feature       | AVL      | Red-Black |
| ------------- | -------- | --------- |
| Height Stored | Yes      | No        |
| Metadata      | Height   | Color     |
| Rotations     | More     | Fewer     |
| Complexity    | O(log n) | O(log n)  |
| Insert Speed  | Slower   | Faster    |

---

# Most Asked Interview Questions

### Why insert RED?

To avoid changing black height.

---

### What determines insertion case?

Color of uncle.

---

### When is recoloring performed?

When uncle is RED.

---

### When is rotation performed?

When uncle is BLACK.

---

### Why does insertion remain O(log n)?

Only one root-to-leaf path is traversed.

---

### Why set root black at end?

Property 2 requires root to be black.

---

# Common Mistakes

### Mistake 1

Forgetting uncle.

---

### Mistake 2

Confusing LR and RL.

---

### Mistake 3

Not recoloring after rotation.

---

### Mistake 4

Forgetting root must be black.

---

# Module Summary

Red-Black insertion:

```text
BST Insert

↓

Color RED

↓

Parent RED?

↓

Check Uncle

↓

RED Uncle
→ Recolor

BLACK Uncle
→ Rotate

↓

Root BLACK
```

This is the complete intuition and algorithm behind Red-Black Tree insertion.
