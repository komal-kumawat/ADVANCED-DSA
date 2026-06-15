# Red-Black Tree Complete Notes

# Module 7: Complete Red-Black Tree Deletion Algorithm

---

# Learning Objectives

By the end of this module, you will understand:

* Full deletion workflow
* BST deletion phase
* FixDelete phase
* How Double Black propagates
* Complete deletion pseudocode
* Production-level intuition
* Interview-level explanation

---

# 1. High-Level Deletion Process

Red-Black deletion consists of:

```text id="j6pkx3"
BST Delete

↓

Identify Deleted Color

↓

If RED Deleted
    Done

If BLACK Deleted
    Fix Double Black

↓

Restore Properties
```

---

# 2. Why RED Node Deletion Is Easy

Example:

```text id="n9s0xz"
      B
     /
    R
```

Delete:

```text id="y8efsu"
R
```

Result:

```text id="nr2yx8"
      B
```

---

Observe:

Black Height:

Before:

```text id="ewhmqo"
1
```

After:

```text id="8m48c5"
1
```

No change.

---

Therefore:

```text id="t5qjrt"
Deleting RED node
=
No fixing required
```

---

# 3. Why BLACK Node Deletion Is Difficult

Example:

```text id="rz4dgo"
      B
     /
    B
```

Delete lower BLACK node.

Result:

```text id="0v4j2i"
      B
```

---

Black count changed.

Property 5 violated.

Need repair.

---

# 4. BST Deletion First

Just like AVL:

---

Case 1

Leaf Node

```text id="skgn4u"
Delete directly
```

---

Case 2

One Child

```text id="j2zwvk"
Child replaces node
```

---

Case 3

Two Children

```text id="aklcja"
Replace with successor
Delete successor
```

---

Only after BST deletion do we apply Red-Black fixes.

---

# 5. Variables Used

Most implementations maintain:

```cpp id="n24d9q"
x
```

Node where Double Black exists.

---

```cpp id="jjp6vh"
sibling
```

Sibling of x.

---

```cpp id="cn7s35"
parent
```

Parent of x.

---

Everything revolves around these.

---

# 6. FixDelete Algorithm

Core idea:

```cpp id="3gf9bo"
while(x != root &&
      x is BLACK)
{
    repair violation
}
```

---

Interpretation:

As long as Double Black exists:

```text id="03ekc9"
Keep fixing
```

---

# 7. Case 1

# Sibling RED

---

Structure

```text id="ghfg4h"
        P(B)
       /   \
     DB    S(R)
          /   \
         B     B
```

---

Observation

If sibling is RED:

```text id="wftmh9"
Parent must be BLACK
```

---

Fix

Swap colors:

```text id="q4iz4j"
Parent ↔ Sibling
```

---

Rotate parent.

---

Result

```text id="w6tb3o"
Sibling becomes BLACK
```

Tree converts into:

```text id="rjaf56"
Sibling BLACK case
```

---

Important

Case 1:

```text id="k0s7k7"
Transforms
does not solve
```

---

# 8. Case 2

# Sibling BLACK

# Two BLACK Children

---

Structure

```text id="h4x7hg"
        P
       / \
     DB   B
         / \
        B   B
```

---

Fix

Color sibling RED.

```text id="blx1eq"
        P
       / \
     DB   R
```

---

Effect

Sibling loses one black.

Double Black moves upward.

```text id="ud3j1e"
Parent becomes Double Black
```

---

Continue upward.

---

# Important Insight

Deletion can travel:

```text id="rgrzv7"
Leaf → Root
```

---

# 9. Case 3

# Sibling BLACK

# Near Child RED

# Far Child BLACK

---

Example

```text id="fsv8mi"
          P
         / \
       DB   B
           /
          R
```

---

Near Child:

```text id="fblc9o"
RED
```

Far Child:

```text id="0khgl7"
BLACK
```

---

Problem

Case 4 requires:

```text id="01ppqj"
Far RED Child
```

---

Therefore convert.

---

Fix

Rotate sibling.

Recolor.

---

Transforms into:

```text id="l3mtql"
Case 4
```

---

# Memory Trick

```text id="l5f5l0"
Case 3
=
Preparation
```

---

# 10. Case 4

# Sibling BLACK

# Far Child RED

---

Example

```text id="gyrj0g"
          P
         / \
       DB   B
             \
              R
```

---

This is the solving case.

---

Fix

Rotate parent.

---

Recolor:

```text id="bb8g6k"
Sibling gets Parent color

Parent becomes BLACK

Far Child becomes BLACK
```

---

Double Black disappears.

---

Deletion complete.

---

# Important

Case 4:

```text id="4qpt1d"
Actually solves
the problem
```

---

# 11. Full Deletion Flow

Deleted node color?

---

RED:

```text id="sagc3n"
Done
```

---

BLACK:

```text id="u0yaz0"
Create Double Black
```

---

Check sibling.

---

RED sibling?

```text id="es57es"
Case 1
```

---

BLACK sibling with BLACK children?

```text id="0sdl2l"
Case 2
```

---

BLACK sibling with Near RED child?

```text id="t1y1o3"
Case 3
```

---

BLACK sibling with Far RED child?

```text id="2e4qq8"
Case 4
```

---

# 12. Deletion Pseudocode

```cpp id="od3fvv"
Delete node as BST

if(deleted node was RED)
    return;

x = replacement node;

while(x != root &&
      x is DOUBLE BLACK)
{
    if(sibling RED)
        Case 1;

    else if(
      sibling BLACK &&
      children BLACK)
        Case 2;

    else if(
      near child RED &&
      far child BLACK)
        Case 3;

    else
        Case 4;
}

root = BLACK;
```

---

# 13. Why Deletion Remains O(log n)

Search:

```text id="wuzpn8"
O(log n)
```

---

BST delete:

```text id="ekrhlm"
O(log n)
```

---

FixDelete:

Maximum:

```text id="2x0n6g"
one root-to-leaf path
```

---

Height:

```text id="kiz3pa"
O(log n)
```

---

Total:

```text id="yw26pp"
Deletion = O(log n)
```

---

# 14. Complete Rotation Usage

Insertion:

```text id="o3f0gx"
LL
RR
LR
RL
```

---

Deletion:

Same rotations.

But triggered by:

```text id="t2w2nq"
Sibling configuration
```

instead of Uncle configuration.

---

# AVL vs Red-Black Deletion

| Feature           | AVL              | Red-Black              |
| ----------------- | ---------------- | ---------------------- |
| Main Issue        | Height imbalance | Black height imbalance |
| Key Concept       | Balance Factor   | Double Black           |
| Metadata          | Height           | Color                  |
| Complexity        | O(log n)         | O(log n)               |
| Coding Difficulty | High             | Very High              |

---

# Most Asked Interview Questions

---

## Q1

Why is deleting RED node easy?

### Answer

Because black height remains unchanged.

---

## Q2

What is Double Black?

### Answer

A conceptual extra blackness used to represent a missing black node after deletion.

---

## Q3

Which deletion case actually fixes the problem?

### Answer

Case 4.

BLACK sibling with Far RED child.

---

## Q4

Why does Case 1 exist?

### Answer

To transform RED sibling into BLACK sibling cases.

---

## Q5

Why can deletion propagate upward?

### Answer

Because Double Black may move to the parent.

---

# Common Mistakes

---

### Mistake 1

Memorizing cases without understanding Double Black.

---

### Mistake 2

Confusing Near Child and Far Child.

---

### Mistake 3

Thinking Case 1 solves the problem.

---

### Mistake 4

Forgetting deletion is sibling-based.

Insertion is uncle-based.

---

# Trainer Notes

Teaching Order:

```text id="3i0w6m"
Why BLACK deletion is difficult

↓

Black Height

↓

Double Black

↓

Sibling

↓

Case 1

↓

Case 2

↓

Case 3

↓

Case 4

↓

Code
```

Never start directly with code.

Students will memorize but not understand.

---

# Module Summary

Deletion difficulty comes from:

```text id="h9nctq"
Loss of a BLACK node
```

which creates:

```text id="pkr4gv"
Double Black
```

The entire algorithm revolves around:

```text id="0rr4lv"
Sibling Cases
```

Case Summary:

```text id="v0nkl8"
Case 1
RED sibling
→ Transform

Case 2
BLACK sibling + BLACK children
→ Push upward

Case 3
BLACK sibling + Near RED
→ Prepare

Case 4
BLACK sibling + Far RED
→ Solve
```

These rules restore all Red-Black properties while maintaining:

```text id="xv5a0r"
O(log n)
```

time complexity.
