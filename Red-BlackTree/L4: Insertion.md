# Red-Black Tree Complete Notes

# Module 4: Red-Black Tree Insertion

# Recoloring, Uncle Concept & Rotations

---

# Learning Objectives

By the end of this module, you will understand:

* Why new nodes are inserted as RED
* Parent, Grandparent, and Uncle concepts
* What property insertion violates
* Recoloring
* Rotation cases
* Complete insertion intuition
* Interview shortcuts
* Why insertion remains O(log n)

---

# 1. The Biggest Interview Question

Almost every interviewer asks:

> Why are new nodes inserted as RED and not BLACK?

Most students memorize:

```text
New nodes are inserted as RED.
```

But do not understand why.

Let's understand deeply.

---

# 2. What Happens If We Insert BLACK?

Consider:

```text
        B
       / \
      B   B
```

Valid Red-Black Tree.

---

Insert:

```text
5
```

as BLACK.

```text
          B
         / \
        B   B
       /
      B
```

---

Let's calculate black heights.

Left path:

```text
B → B → B → NIL
```

Black Count:

```text
3
```

Right path:

```text
B → B → NIL
```

Black Count:

```text
2
```

Property 5 breaks immediately.

---

# Problem

Inserting BLACK changes black height.

Black height is extremely sensitive.

---

# 3. What Happens If We Insert RED?

Same tree:

```text
        B
       / \
      B   B
```

Insert RED:

```text
          B
         / \
        B   B
       /
      R
```

---

Black count:

Left path:

```text
B → B → NIL
```

Black Nodes:

```text
2
```

Right path:

```text
B → B → NIL
```

Black Nodes:

```text
2
```

Still valid.

---

# Important Observation

RED nodes do not contribute to black height.

Therefore:

```text
Insert RED
=
Minimum disturbance
```

This is why every Red-Black Tree insertion starts with:

```text
New Node = RED
```

---

# Interview Answer

### Why are new nodes inserted as RED?

Because inserting BLACK changes black height immediately and violates Property 5.

RED insertion preserves black height.

---

# 4. What Properties Can Break?

After inserting RED:

Properties 1,2,3,5 remain valid.

Usually only:

```text
Property 4
```

can break.

---

Property 4:

```text
No RED node can have a RED child.
```

---

Example:

```text
      B
       \
        R
         \
          R
```

Red parent.

Red child.

Violation.

---

# Great Insight

Red-Black insertion focuses almost entirely on fixing:

```text
RED - RED violations
```

---

# 5. Parent, Grandparent, Uncle

To repair violations we need three relatives.

---

Suppose:

```text
           G
          /
         P
        /
       X
```

Where:

```text
G = Grandparent
P = Parent
X = Inserted Node
```

---

Now add the sibling of parent:

```text
           G
          / \
         P   U
        /
       X
```

This node is called:

```text
U = Uncle
```

---

These three nodes drive the entire insertion algorithm.

---

# Interview Trick

Whenever insertion causes a violation:

Immediately identify:

```text
Node
Parent
Grandparent
Uncle
```

before thinking further.

---

# 6. Case Classification

Insertion problems are classified by:

```text
Color of Uncle
```

Two major categories:

---

Case A

```text
Uncle = RED
```

---

Case B

```text
Uncle = BLACK
```

Everything comes from these.

---

# CASE A

# Uncle is RED

Most students call this:

```text
Recoloring Case
```

---

# Example

```text
             B
           /   \
          R     R
         /
        R
```

Let's label:

```text
Grandparent = B

Parent = Left R

Uncle = Right R

Node = Bottom R
```

---

Violation:

```text
RED Parent
RED Child
```

---

# Step 1

Parent becomes BLACK.

```text
             B
           /   \
          B     R
         /
        R
```

---

# Step 2

Uncle becomes BLACK.

```text
             B
           /   \
          B     B
         /
        R
```

---

# Step 3

Grandparent becomes RED.

```text
             R
           /   \
          B     B
         /
        R
```

---

Result:

```text
Property 4 fixed locally
```

---

But now:

Grandparent may violate higher levels.

Thus we continue upward.

---

# Recoloring Rule

If:

```text
Parent = RED

Uncle = RED
```

Then:

```text
Parent -> BLACK

Uncle -> BLACK

Grandparent -> RED
```

---

# Easy Memory Trick

```text
RED Uncle

=
Color Flip
```

No rotation.

Only recoloring.

---

# CASE B

# Uncle is BLACK

Now rotations become necessary.

---

Why?

Because recoloring alone cannot repair structure.

---

# Four Possibilities

1. LL
2. RR
3. LR
4. RL

Exactly like AVL.

---

# 7. LL Case

Structure:

```text
            B
           /
          R
         /
        R
```

Labels:

```text
Grandparent = Top B

Parent = Middle R

Node = Bottom R
```

Uncle:

```text
BLACK (or NIL)
```

---

# Fix

Right Rotation.

---

Before:

```text
            G(B)
           /
         P(R)
         /
       X(R)
```

---

After:

```text
          P(B)
         /   \
       X(R)  G(R)
```

---

Important:

Swap colors:

```text
Parent ↔ Grandparent
```

---

# LL Rule

```text
Uncle = BLACK

Node in Left subtree of Parent

Parent in Left subtree of Grandparent
```

Perform:

```text
Right Rotation
```

---

# 8. RR Case

Mirror image.

---

Before:

```text
G(B)
  \
   P(R)
      \
       X(R)
```

---

After:

```text
        P(B)
       /   \
    G(R)   X(R)
```

---

Rotation:

```text
Left Rotation
```

---

# RR Rule

```text
Right
Right
```

Apply:

```text
Left Rotation
```

---

# 9. LR Case

Before:

```text
        G(B)
       /
      P(R)
         \
          X(R)
```

---

Notice:

```text
Left
then
Right
```

---

Single rotation won't work.

Need two.

---

Step 1

Left Rotate Parent.

```text
        G
       /
      X
     /
    P
```

---

Step 2

Right Rotate Grandparent.

```text
          X
         / \
        P   G
```

---

# LR Rule

```text
Left
Right
```

Apply:

```text
Left Rotate

Right Rotate
```

---

# 10. RL Case

Mirror image.

Before:

```text
       G
         \
          P
         /
        X
```

---

Step 1

Right Rotate Parent.

---

Step 2

Left Rotate Grandparent.

---

Result:

```text
         X
        / \
       G   P
```

---

# RL Rule

```text
Right
Left
```

Apply:

```text
Right Rotate

Left Rotate
```

---

# 11. Insertion Decision Tree

This is the interview cheat code.

---

Question:

```text
Parent Color?
```

---

If:

```text
BLACK
```

Done.

Tree already valid.

---

If:

```text
RED
```

Violation.

Now ask:

```text
Uncle Color?
```

---

If:

```text
RED
```

Do:

```text
Recolor
```

---

If:

```text
BLACK
```

Do:

```text
Rotation
```

(LL, RR, LR, RL)

---

# 12. Why Rotations Fix the Problem

Rotations:

1. Reduce tree height locally.
2. Break RED-RED violation.
3. Preserve BST property.
4. Preserve black height.

---

# 13. Why Insertion is O(log n)

Insertion path:

```text
Root → Leaf
```

Length:

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

Total:

```text
Insertion = O(log n)
```

---

# 14. Interview Questions

---

## Q1

Why insert RED?

### Answer

RED insertion does not change black height.

BLACK insertion does.

---

## Q2

What is Uncle?

### Answer

Sibling of the parent.

---

## Q3

When do we recolor?

### Answer

When Uncle is RED.

---

## Q4

When do we rotate?

### Answer

When Uncle is BLACK.

---

## Q5

How many rotation cases exist?

### Answer

```text
LL
RR
LR
RL
```

---

## Q6

Which property usually breaks after insertion?

### Answer

Property 4.

```text
No consecutive RED nodes.
```

---

# Common Mistakes

---

## Mistake 1

Thinking insertion breaks black height.

Usually it doesn't.

New node is RED.

---

## Mistake 2

Forgetting to check Uncle.

Uncle determines the case.

---

## Mistake 3

Applying rotation when recoloring is enough.

---

## Mistake 4

Not swapping colors after rotation.

---

# Trainer Notes

Teach insertion in this order:

1. Why RED insertion?
2. RED-RED violation.
3. Parent-Grandparent-Uncle.
4. RED Uncle case.
5. BLACK Uncle case.
6. LL/RR.
7. LR/RL.
8. Full algorithm.

Students learn much faster this way.

---

# Module Summary

Insertion starts by adding:

```text
RED node
```

If:

```text
Parent BLACK
```

Done.

If:

```text
Parent RED
```

Check Uncle.

---

If Uncle is:

```text
RED
```

Use:

```text
Recoloring
```

---

If Uncle is:

```text
BLACK
```

Use:

```text
Rotations
```

(LL, RR, LR, RL)

This is the complete intuition behind Red-Black Tree insertion.
