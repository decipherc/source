.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Binary Search Trees (BST)
=========================

A binary search tree is a type of binary tree whose nodes of the left sub-tree are less than that node's value and the nodes of the right sub-tree are greater than that node's value.

.. code-block:: c++

            5
      3          8
   1     4          10

.. code-block:: c++

             Marshall
        Lily       Robin    Ted
   Barney

Here is the interface of a BST:

- Search it for a value
- Insert an item
- Delete an item
- Find the height
- Traverse it
- Free the memory

Searching a BST:

Algorithm:
1. Start at the root of the tree
2. While a node is not nullptr
     a. If the inputted value is equal to the current node's value, return true
     b. If it is less than the current node's value, go left
     c. If it is greater than the current node's value, go right
3. If the node is nullptr, return false

Code Iterative Version:

.. code-block:: c++

   bool search(int value, Node *ptr)
   {
        while (ptr != nullptr)
        {
             if (value == ptr->value)
                  return true;
             else if (value < ptr->value)
                  ptr = ptr->left;
             else
                  ptr = ptr->right;
        }
        return false;
   }

Code Recursive Version:

.. code-block:: c++

   bool search(int value, Node *ptr)
   {
        if (ptr == nullptr)
             return false;
        else if (value == ptr->value)
             return true;
        else if (value < ptr->value)
             return (search(value, ptr->left));
        else
             return (search(value, ptr->right));
   }

.. code-block:: c++

           d
       b        e
   a       c

Example:

.. code-block:: c++

   Search for c
   c < d, go left
        Node is c
        c > b, go right
             Node is c
             Return true

Example:

.. code-block:: c++

   Search for e
   e > d, go right
        Node is e
        Return true

Example:

.. code-block:: c++

   Search for f
   f > d, go right
        Node is f
        f > e, go right
             Node is nullptr
             Return false

The average big-O of this search, for a BSt with n values, is ``O(log(n))``.
The worst case big-O is ``O(n)``.

Worst case:

.. code-block:: c++

   z
        y
             x
                  w
                       v
                            u
                                 etc.

Inserting an item into a BST:

Algorithm:

1. If the tree is empty
     a. Allocate a new node and put the inputted value in
     b. Point the root pointer to the new node
     c. Return
2. Start at the root of the tree
3. While we haven't returned
     a. If the value is equal to the current node's value, return
     b. If the value is less than the current node's value
          i. If there is a left child, go left
          ii. Otherwise allocate a new node and put the value in it. Set the current node's left pointer to the new node
     c. If the value is greater than the current node's value
          i. If there is a right child, go right
          ii. Otherwise allocate a new node and put the value in it. Set the current node's right pointer to the new node

Code:

.. code-block:: c++

   void insert (const string &value)
   {
        if (root == nullptr)
        {
             root = new Node(value);
             return;
        }
        Node *cur = root;
        for (;;)
        {
             if (value == cur->value)
                  return;
             if (value < cur->value)
             {
                  if (cur->left != nullptr)     // Go left
                       cur = cur->left;
                  else
                  {
                       cur->left = new Node(value);
                       return;
                  }
             }
             else if (value > cur->value)
             {
                  if (cur->right != nullptr)     // Go right
                       cur = cur->right;
                  else
                  {
                       cur->right = new Node(value);
                       return;
                  }
        }
   }

The average big-O of insertion is ``O(log(n))`` (to find the right place), the worst case is ``O(n)``.

You want your BST to be busty and balanced, like a Christmas tree, not a line.

Good:

.. code-block:: c++

           d
       b        e
   a       c

Bad:

.. code-block:: c++

   e
        d
             c
                  b
                       a

.. code-block:: c++

        b
   a         c
                  d
                       e

Finding the minimum value of a BST (left-most node):

Code Iterative Version:

.. code-block:: c++

   int min(Node *ptr)
   {
        if (ptr == nullptr)
             return -1;     // empty tree
   
        while (ptr->left != nullptr)     // Keep on going left until you get to the left-most
             ptr = ptr->left;
   
        return (ptr->value);
   }

Code Recursive Version:

.. code-block:: c++

   int min(Node *ptr)
   {
        if (ptr == nullptr)
             return -1;     // empty tree
   
        if (ptr->left == nullptr)     // Stop iterating when you get to the left-most
             return (ptr->value);
   
        return (min(ptr->left));
   }

Finding the maximum value of a BST (right-most node):

Code Iterative Version:

.. code-block:: c++

   int max(Node *ptr)
   {
        if (ptr == nullptr)
             return -1;     // empty tree
   
        while (ptr->right != nullptr)     // Keep on going right until you get to the right-most
             ptr = ptr->right;
   
        return (ptr->value);
   }

Code Recursive Version:

.. code-block:: c++

   int max(Node *ptr)
   {
        if (ptr == nullptr)
             return -1;     // empty tree
   
        if (ptr->right == nullptr)     // Stop iterating when you get to the right-most
             return (ptr->value);
   
        return (max(ptr->right));
   }

Printing out the values of a BST in alphabetical order:
This is same as traversing a binary tree with an in-order traversal.

.. code-block:: c++

   // In-order traversal
   void print(Node *cur)
   {
        if (cur == nullptr)
             return;
        print(cur->left);
        cout << cur->value << endl;
        print(cur->right);
   }

Big-o of this is ``O(n)``.

Freeing all of the memory of the tree:
This is the same as traversing a binary tree with post-order traversal

.. code-block:: c++

   // Post-order traversal
   void free(Node *cur)
   {
        if (cur == nullptr)
             return;
        free(cur->left);
        free(cur->right);
        delete cur;
   }

This is also ``O(n)``.

Deleting a node from a BST:
This is more complicated than you may think. There are two major steps.

Algorithm:
1. Find the value in the tree with a standard BST search, but use two pointers: cur and parent. Cur should point to the node you want to delete, parent should point to that node's parent.
2. If the node is found, determine what case it falls into:
     a. The node is a leaf
     b. The node has one child
     c. The node has two children
3. If case a,
     a. If cur is the root node, set the root pointer to nulltpr
     b. Otherwise set either the parent's left or right pointer nullptr (depends on whether cur is the left or right child)
     c. Delete cur
4. If case b,
     a. Create a grandchild pointer and set it to either cur's left or right pointer (depends on whether it has a left or a right child)
     b. If cur is the root node, set the root pointer to the grandchild
     b. Otherwise link either the parent's left or right pointer to the grandchild (depends on whether cur is the left or right child)
     c. Delete cur
5. If case c,
     a. Find either the largest child (right-most child) of the value's left sub-tree or the smallest child (left-most child) of the right sub-tree
     b. Copy that node's value into temp
     c. Remove that node
     d. Copy temp into the node you wish to delete

.. code-block:: c++

   void delete(Node *cur)
   {
        Node* parent = nullptr;
        cur = root;
        while (cur != nullptr)
        {
             if (value == cur->value)
                  break;     // Found the value
             if (value < cur->value)
             {
                  parent = cur;
                  cur = cur->left;
             }
             else if (value > cur->value)
             {
                  parent = cur;
                  cur = cur->right;
             }
        }
        if (cur->left == nullptr && cur->right == nullptr)     // Case a
        {
             if (parent == root)
                  root = nullptr;
             else if (parent->left == cur)
                  parent->left = nullptr;
             else
                  parent->right = nullptr;
             delete cur;
        }
   
        else if (cur->left != nullptr || cur->right != nullptr)     // Case b
        {
             Node *grandchild;
             if (cur->left != nullptr)
                  grandchild = cur->left;
             else
                  grandchild = cur->right;
             if (parent == cur)
                  root = grandchild;
             else if (parent->left == cur)
                  parent->left = grandchild;
             else if (parent->right == cur)
                  parent->right = grandchild;
             delete cur;
        }
   
        else if (cur->left != nullptr && cur->right != nullptr)     // Case c
        {
             Node *ptr;
             if (cur->left != nullptr)
             {
                  ptr = cur->left;
                  while (ptr->right != nullptr)
                       ptr = ptr->right;
             }
             else     // If there isn't a left child
             {
                  ptr = cur->right;
                  while (ptr->left != nullptr)
                       ptr = ptr->left;
             }
             int temp = ptr->value;
             if (ptr->left == nullptr && ptr->right == nullptr)
                  // Delete it with case a
             if (ptr->left != nullptr || ptr->right != nullptr)
                  // Delete it with case b
             cur->value = temp;
        }
   }