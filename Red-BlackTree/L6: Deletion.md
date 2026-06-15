# Red-Black Tree Complete Notes

# Module 6: Red-Black Tree Deletion

# Double Black Concept, Cases & Intuition

---

# Learning Objectives

By the end of this module, you will understand:

* Why deletion is harder than insertion
* Why Red-Black deletion is considered one of the hardest tree algorithms
* The Double Black concept
* How deletion affects black height
* Sibling-based deletion cases
* Recoloring and rotation logic
* Interview-level understanding of deletion

---

# 1. Why Is Red-Black Deletion Hard?

Interview Question:

> Which is harder: Red-Black insertion or Red-Black deletion?

Answer:

```text id="n4bxh1"
Red-Black Deletion
```

---

# Why?

Insertion usually breaks:

```text id="pwgjvt"
Property 4
(No consecutive RED nodes)
```

Only one major property.

---

Deletion can break:

```text id="s52jq5"
Property 2
Property 4
Property 5
```

especially:

```text id="v7b8b3"
Black Height
```

---

# The Real Problem

Insertion:

```text id="lmepmh"
Adds a RED node
```

RED nodes do not affect black height.

Easy.

---

Deletion:

May remove:

```text id="s99l3x"
BLACK node
```

This changes black height.

Dangerous.

---

# 2. Understanding the Black Height Problem

Consider:

```text id="qf55dz"
        B
       / \
      B   B
```

Valid.

---

Black height:

Left path:

```text id="vr7ccf"
2
```

Right path:

```text id="36e3aj"
2
```

Balanced.

---

Delete left BLACK node:

```text id="8hxxlq"
        B
          \
           B
```

Now:

Left path:

```text id="2dw1qa"
1
```

Right path:

```text id="v8v6dr"
2
```

Property 5 violated.

---

# Key Insight

Deleting BLACK node creates:

```text id="4mjlwm"
Black Deficiency
```

One side has fewer black nodes.

We must repair it.

---

# 3. The Double Black Concept

This is the most important deletion idea.

---

Imagine:

```text id="7rvvru"
BLACK node removed
```

Instead of thinking:

```text id="6e4vr9"
One black disappeared
```

we think:

```text id="4efiqv"
Child inherits extra blackness
```

This extra blackness is called:

```text id="mj0xql"
DOUBLE BLACK
```

---

# Why Invent Double Black?

Without it:

Balancing becomes extremely difficult.

Double Black allows us to:

```text id="jlwmr8"
Track missing black height
```

during repair.

---

# Example

Original:

```text id="ffy7yf"
      B
     /
    B
```

Delete lower BLACK node.

Instead of:

```text id="mjlwmh"
Black count decreased
```

Think:

```text id="5ysgcu"
NIL becomes Double Black
```

Representation:

```text id="0urx8q"
      B
     /
   DB
```

(DB = Double Black)

---

# Goal of Deletion

Remove:

```text id="s1sqk0"
Double Black
```

without violating Red-Black properties.

---

# 4. Cases Based on Sibling

Insertion focused on:

```text id="m2ezjk"
Uncle
```

Deletion focuses on:

```text id="8ztg4y"
Sibling
```

---

Important relatives:

```text id="w34kdl"
Node (Double Black)

Parent

Sibling
```

---

Example:

```text id="1uym4f"
        P
       / \
     DB   S
```

S:

```text id="mjlwmu"
Sibling
```

---

Almost every deletion case depends on sibling color.

---

# 5. Case 1: Sibling is RED

This is easiest to recognize.

---

Example

```text id="6gx4mh"
         B
        / \
      DB   R
          / \
         B   B
```

Sibling:

```text id="f0kv1z"
RED
```

---

Observation

Red sibling means:

```text id="8rzv9w"
Parent must be BLACK
```

because two consecutive reds are forbidden.

---

Fix

Swap colors:

```text id="q4e9gc"
Parent ↔ Sibling
```

Then rotate.

---

After rotation:

Sibling becomes parent.

Tree transforms into a case where sibling is BLACK.

---

# Why?

Case 1 is never the final solution.

It converts the tree into another deletion case.

---

# Interview Shortcut

```text id="jlwmrw"
RED sibling
=
Transform problem
```

Not solve problem.

---

# 6. Case 2: Sibling BLACK with Two BLACK Children

Example:

```text id="9a9x0e"
          P
         / \
       DB   B
           / \
          B   B
```

Sibling:

```text id="f0l02k"
BLACK
```

Children:

```text id="ub0e7r"
BLACK
BLACK
```

---

Fix

Color sibling RED.

```text id="w15yhs"
          P
         / \
       DB   R
```

---

Effect

Sibling loses one black.

Double Black moves upward.

Now:

```text id="5tvq5w"
Parent becomes Double Black
```

---

This is called:

```text id="jlwmrx"
Push Double Black Upward
```

---

# Why?

No rotation needed.

Only recoloring.

---

# Important Insight

Deletion can propagate upward.

Insertion usually stops quickly.

Deletion may continue all the way to root.

---

# 7. Case 3: Sibling BLACK

# Near Child RED

# Far Child BLACK

This is the most confusing case.

---

Assume:

```text id="7dxk7r"
DB on left side
```

Example:

```text id="4o0w9k"
           P
          / \
        DB   B
            /
           R
```

---

Sibling:

```text id="53t0f7"
BLACK
```

Near child:

```text id="jlwms0"
RED
```

Far child:

```text id="jlwms1"
BLACK
```

---

Fix

Rotate sibling.

Recolor.

Transform into Case 4.

---

# Important

Case 3 never directly solves deletion.

It prepares for Case 4.

---

# Memory Trick

```text id="jlwms2"
Case 3
=
Preparation Case
```

---

# 8. Case 4: Sibling BLACK

# Far Child RED

This is the final solving case.

---

Example

```text id="jlwms3"
           P
          / \
        DB   B
              \
               R
```

---

Sibling:

```text id="jlwms4"
BLACK
```

Far child:

```text id="jlwms5"
RED
```

---

Fix

Rotate parent.

Recolor.

Remove Double Black.

---

Result

All properties restored.

Deletion ends.

---

# Memory Trick

```text id="jlwms6"
Far RED Child
=
Problem Solved
```

---

# 9. Left and Right Symmetry

Everything discussed assumed:

```text id="jlwms7"
Double Black on left
```

---

If Double Black is on right:

Mirror all cases.

---

Interview Tip:

Learn only one side.

Mirror mentally.

---

# 10. Why Does Double Black Work?

Suppose:

```text id="jlwms8"
Black node removed
```

Missing:

```text id="jlwms9"
1 black
```

Double Black represents:

```text id="jlwmta"
Extra black requirement
```

that must be redistributed.

---

Think of it as:

```text id="jlwmtb"
Debt
```

Tree owes one black node.

Deletion fixes the debt.

---

# 11. Visual Summary

---

Delete node.

---

Deleted node color?

```text id="jlwmtc"
RED
```

Done.

Easy.

---

Deleted node color?

```text id="jlwmtd"
BLACK
```

Create:

```text id="ljive"
Double Black
```

---

Check sibling.

---

Sibling RED?

```text id="jlwmtf"
Rotate
Recolor
Convert case
```

---

Sibling BLACK + Two BLACK Children?

```text id="jlwmtg"
Push Double Black Up
```

---

Sibling BLACK + Near RED Child?

```text id="jlwmth"
Convert to Case 4
```

---

Sibling BLACK + Far RED Child?

```text id="jlwmti"
Rotate
Recolor
Done
```

---

# 12. Why Deletion is O(log n)

Search node:

```text id="jlvmtj"
O(log n)
```

---

Double Black propagation:

```text id="jlvmtk"
At most root height
```

---

Height:

```text id="jlvmtl"
O(log n)
```

---

Rotations:

```text id="jlvmtm"
O(1)
```

---

Total:

```text id="jlvmtn"
Deletion = O(log n)
```

---

# AVL vs Red-Black Deletion

| Feature      | AVL              | Red-Black              |
| ------------ | ---------------- | ---------------------- |
| Main Problem | Height imbalance | Black height imbalance |
| Concept      | Balance Factor   | Double Black           |
| Rotations    | Several          | Fewer                  |
| Complexity   | O(log n)         | O(log n)               |
| Difficulty   | High             | Very High              |

---

# Interview Questions

---

## Q1

Why is Red-Black deletion harder than insertion?

### Answer

Deletion can violate black height and create Double Black situations.

---

## Q2

What is Double Black?

### Answer

A conceptual state representing a missing black node after deletion.

---

## Q3

Which relative is most important during deletion?

### Answer

Sibling.

---

## Q4

What happens when sibling is RED?

### Answer

Rotate and recolor to transform into another case.

---

## Q5

What happens when sibling is BLACK with two BLACK children?

### Answer

Push Double Black upward.

---

## Q6

Which case actually resolves Double Black?

### Answer

Case 4 (BLACK sibling with far RED child).

---

# Common Mistakes

### Mistake 1

Trying to memorize deletion without understanding Double Black.

---

### Mistake 2

Confusing near child and far child.

---

### Mistake 3

Thinking Case 1 solves deletion.

It only transforms the problem.

---

### Mistake 4

Not recognizing mirror cases.

---

# Trainer Notes

Teach deletion in this order:

1. Black height problem
2. Missing black node
3. Double Black
4. Sibling concept
5. Four sibling cases
6. Rotations and recoloring

Never start with code.

Students will only memorize.

---

# Module Summary

Deletion is difficult because removing a BLACK node changes:

```text id="jlwmto"
Black Height
```

To track the missing black node we create:

```text id="jlwmtp"
Double Black
```

Deletion repair revolves around:

```text id="jlwmtq"
Sibling
```

Cases:

```text id="jlwmtr"
Sibling RED

Sibling BLACK + BLACK Children

Sibling BLACK + Near RED Child

Sibling BLACK + Far RED Child
```

These cases eventually eliminate Double Black and restore all Red-Black properties.


# Red-Black Tree Deletion — Conceptual Questions & Practice Exercises

---

# Conceptual Questions

## 1. Why is Deletion Harder than Insertion?

Deletion is harder because:

### During Insertion
- A newly inserted node is always RED.
- No black node is removed.
- Black Height usually remains unchanged.
- We mainly deal with RED-RED violations.

### During Deletion
- A BLACK node may be removed.
- Removing a BLACK node can decrease Black Height on one path.
- This violates Red-Black Tree properties.
- We must restore balance while preserving Black Height.

### Example

Before deletion:

```text
      20(B)
     /     \
  10(B)   30(B)
```

Black Height of both paths = 2

Delete 10(B):

```text
      20(B)
          \
         30(B)
```

Left path has fewer black nodes than right path.

Black Height property breaks.

This is why deletion is more complicated.

---

## 2. What is Double Black?

Double Black is a conceptual marker used during deletion.

It represents:

```text
Missing Black Node + Existing Black Node
```

or

```text
BLACK + BLACK = DOUBLE BLACK
```

It is NOT an actual color.

We use it temporarily to indicate:

> "This subtree has one extra black deficiency."

### Example

Delete black leaf:

```text
      20(B)
     /
   10(B)
```

Delete 10(B)

```text
      20(B)
     /
   NULL(DB)
```

NULL becomes Double Black.

Now we must eliminate this Double Black.

---

## 3. Why Does Deleting a RED Node Usually Require No Fix?

Because RED nodes do not contribute to Black Height.

### Example

Before:

```text
      20(B)
     /
   10(R)
```

Black count:

```text
20(B)
```

Delete 10(R)

```text
      20(B)
```

Black count remains unchanged.

No Red-Black property is violated.

Therefore:

```text
Deleting RED node
=
No balancing needed
```

---

## 4. Why is Sibling More Important than Uncle During Deletion?

### During Insertion

We focus on:

```text
Parent
Grandparent
Uncle
```

because the violation is:

```text
RED parent + RED child
```

Uncle determines which insertion case occurs.

---

### During Deletion

The problem is:

```text
DOUBLE BLACK
```

To remove Double Black, we need help from the nearby subtree.

That nearby subtree is the:

```text
Sibling
```

Sibling can:

- Donate a black node
- Rotate
- Recolor

Therefore deletion decisions depend almost entirely on sibling color and sibling's children.

---

## 5. Which Deletion Case Actually Resolves the Problem?

### Case 4

Sibling is BLACK and far child is RED.

Example:

```text
        P(B)
       /    \
    DB      S(B)
              \
              R(R)
```

Perform:

- Rotation
- Recoloring

Result:

```text
Double Black removed
Black Height restored
Algorithm stops
```

This is the only case that directly eliminates Double Black immediately.

---

## 6. Why Can Deletion Propagate Upward?

Sometimes Double Black cannot be removed locally.

### Example

```text
        P(B)
       /    \
     DB    S(B)
          /   \
       B      B
```

Sibling has:

```text
BLACK sibling
BLACK near child
BLACK far child
```

We recolor sibling RED:

```text
        P(B)
       /    \
     DB    S(R)
```

Now:

```text
Parent loses one black contribution
```

So Double Black moves upward:

```text
P becomes Double Black
```

This process may continue toward the root.

Thus deletion can propagate upward multiple levels.

---

# Coding Exercises

---

## Exercise 1: Draw All Four Sibling Cases

### Case 1 — Red Sibling

```text
        P(B)
       /    \
     DB     S(R)
           /   \
        B      B
```

---

### Case 2 — Black Sibling, Both Children Black

```text
        P(?)
       /    \
     DB     S(B)
           /   \
        B      B
```

---

### Case 3 — Black Sibling, Near Child Red

```text
        P(?)
       /    \
     DB     S(B)
           /
        R(R)
```

---

### Case 4 — Black Sibling, Far Child Red

```text
        P(?)
       /    \
     DB     S(B)
               \
               R(R)
```

---

## Exercise 2: Trace Deletion Manually

Given:

```text
          20(B)
         /     \
      10(B)   30(B)
```

### Step 1

Delete:

```text
10(B)
```

Result:

```text
          20(B)
         /     \
      DB      30(B)
```

---

### Step 2

Identify:

```text
Sibling = 30(B)
```

Sibling's children:

```text
BLACK
BLACK
```

Case 2

---

### Step 3

Recolor sibling:

```text
30(B) → 30(R)
```

Move Double Black upward.

---

### Step 4

Root absorbs Double Black.

Final:

```text
          20(B)
                \
                30(R)
```

---

## Exercise 3: Identify Near Child vs Far Child

Consider:

```text
        P
       / \
     DB   S
         / \
        A   B
```

### Near Child

Closer to Double Black.

```text
A
```

---

### Far Child

Farther from Double Black.

```text
B
```

---

### Opposite Side Example

```text
        P
       / \
      S   DB
     / \
    A   B
```

Near Child:

```text
B
```

Far Child:

```text
A
```

---

## Exercise 4: Explain Double Black Without Using Code

Imagine a company requires:

```text
2 security guards per gate.
```

Every gate currently has:

```text
2 guards
```

One guard suddenly leaves.

Now one gate has:

```text
1 guard
```

To indicate the shortage, management marks that gate:

```text
"Needs One More Guard"
```

This temporary warning label is like Double Black.

The gate itself is not special.

It simply indicates:

```text
A missing black node.
```

The tree then redistributes guards (rotations/recoloring) until every gate again has the required number.

---

## Exercise 5: Simulate Complete Deletion

Initial Tree:

```text
            20(B)
           /     \
        10(B)   30(B)
                 \
                 40(R)
```

Delete:

```text
10(B)
```

---

### Step 1

Create Double Black:

```text
            20(B)
           /     \
         DB     30(B)
                  \
                  40(R)
```

---

### Step 2

Sibling:

```text
30(B)
```

Far child:

```text
40(R)
```

Case 4 detected.

---

### Step 3

Left Rotation around 20

```text
            30(B)
           /     \
        20(B)   40(B)
```

---

### Step 4

Double Black disappears.

Tree satisfies all Red-Black properties.

Deletion complete.

---

# Interview Quick Revision

### When No Fix is Needed

```text
Delete RED node
Delete node with RED child
```

---

### When Double Black Appears

```text
Delete BLACK leaf
Delete BLACK node replaced by NULL
```

---

### Four Deletion Cases

```text
1. Red Sibling
2. Black Sibling + Black Children
3. Black Sibling + Near Child Red
4. Black Sibling + Far Child Red
```

---

### Case That Ends Immediately

```text
Case 4
```

---

### Case That Moves Upward

```text
Case 2
```

---

### Most Important Node During Deletion

```text
Sibling
```

---

### Most Important Concept

```text
Double Black
```

because every deletion fix-up is ultimately about removing the Double Black while maintaining Black Height.