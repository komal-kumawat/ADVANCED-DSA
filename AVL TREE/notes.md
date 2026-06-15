# AVL Tree - Interview Revision Notes + Code

---

# AVL Tree Definition

AVL Tree is a self-balancing Binary Search Tree (BST) where:

```cpp
Balance Factor = Height(Left) - Height(Right)
```

For every node:

```cpp
-1 <= BF <= 1
```

---

# AVL Node Structure

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

# Height Function

```cpp
int getHeight(Node* root)
{
    if(root == NULL)
        return 0;

    return root->height;
}
```

Time Complexity:

```cpp
O(1)
```

---

# Balance Factor Function

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

# Right Rotation (LL Case)

Before:

```text
        y
       /
      x
     / \
    A   B
```

After:

```text
        x
       / \
      A   y
         /
        B
```

Code:

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

# Left Rotation (RR Case)

Before:

```text
    x
     \
      y
     / \
    B   C
```

After:

```text
        y
       / \
      x   C
       \
        B
```

Code:

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

# Rotation Summary

| Case | Pattern     | Rotation                   |
| ---- | ----------- | -------------------------- |
| LL   | Left Left   | Right Rotate               |
| RR   | Right Right | Left Rotate                |
| LR   | Left Right  | Left Rotate + Right Rotate |
| RL   | Right Left  | Right Rotate + Left Rotate |

---

# AVL Insertion

Algorithm:

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

Code:

```cpp
Node* insert(Node* root, int key)
{
    // BST insertion

    if(root == NULL)
        return new Node(key);

    if(key < root->key)
        root->left =
            insert(root->left, key);

    else if(key > root->key)
        root->right =
            insert(root->right, key);

    else
        return root;

    // Update Height

    root->height =
        1 +
        max(getHeight(root->left),
            getHeight(root->right));

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

Complexity:

```cpp
O(log n)
```

---

# Minimum Value Node

Used during deletion.

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

Complexity:

```cpp
O(log n)
```

---

# AVL Deletion

Algorithm:

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
```

Code:

```cpp
Node* deleteNode(Node* root, int key)
{
    if(root == NULL)
        return root;

    if(key < root->key)
    {
        root->left =
            deleteNode(root->left, key);
    }

    else if(key > root->key)
    {
        root->right =
            deleteNode(root->right, key);
    }

    else
    {
        // One child or no child

        if(root->left == NULL ||
           root->right == NULL)
        {
            Node* temp =
                root->left ?
                root->left :
                root->right;

            if(temp == NULL)
            {
                temp = root;
                root = NULL;
            }
            else
            {
                *root = *temp;
            }

            delete temp;
        }

        // Two children

        else
        {
            Node* temp =
                minValueNode(root->right);

            root->key = temp->key;

            root->right =
                deleteNode(root->right,
                           temp->key);
        }
    }

    if(root == NULL)
        return root;

    // Update Height

    root->height =
        1 +
        max(getHeight(root->left),
            getHeight(root->right));

    int balance =
        getBalance(root);

    // LL

    if(balance > 1 &&
       getBalance(root->left) >= 0)
    {
        return rightRotate(root);
    }

    // LR

    if(balance > 1 &&
       getBalance(root->left) < 0)
    {
        root->left =
            leftRotate(root->left);

        return rightRotate(root);
    }

    // RR

    if(balance < -1 &&
       getBalance(root->right) <= 0)
    {
        return leftRotate(root);
    }

    // RL

    if(balance < -1 &&
       getBalance(root->right) > 0)
    {
        root->right =
            rightRotate(root->right);

        return leftRotate(root);
    }

    return root;
}
```

Complexity:

```cpp
O(log n)
```

---

# Complexity Table

| Operation          | Complexity |
| ------------------ | ---------- |
| Search             | O(log n)   |
| Insert             | O(log n)   |
| Delete             | O(log n)   |
| Rotation           | O(1)       |
| Height Calculation | O(1)       |

---

# AVL vs BST

| Feature | BST  | AVL      |
| ------- | ---- | -------- |
| Height  | O(n) | O(log n) |
| Search  | O(n) | O(log n) |
| Insert  | O(n) | O(log n) |
| Delete  | O(n) | O(log n) |
| Balance | No   | Yes      |

---

# Most Asked Interview Questions

### What is AVL Tree?

Self-balancing BST with:

```cpp
-1 <= BF <= 1
```

---

### What is Balance Factor?

```cpp
Height(Left) - Height(Right)
```

---

### Why do we store height?

To compute BF in O(1).

---

### How many rotations are there?

```text
LL
RR
LR
RL
```

---

### Which rotation is used for LL?

```text
Right Rotation
```

---

### Which rotation is used for RR?

```text
Left Rotation
```

---

### Why is AVL search O(log n)?

AVL guarantees:

```cpp
Height = O(log n)
```

---

### Which STL container uses AVL Tree?

None.

C++ STL:

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

internally.

---

# 1-Minute AVL Revision

```text
AVL = Self Balancing BST

BF = Height(left)-Height(right)

Allowed BF:
-1,0,+1

LL -> Right Rotate
RR -> Left Rotate
LR -> Left + Right Rotate
RL -> Right + Left Rotate

Search  -> O(log n)
Insert  -> O(log n)
Delete  -> O(log n)

Rotation -> O(1)

Height <= 1.44 log₂(n+2)
```
