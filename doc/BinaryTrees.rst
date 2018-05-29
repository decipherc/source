.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Binary Trees
============

A tree is a type of linked data structure of nodes.
Every node has multiple pointers that point to other nodes (or nullptr).
The whole structure is pointed to with a root pointer, which points to the root node.
If a node points to another, the node it points to is the child node and the node itself is the parent node.
If a tree doesn't have any nodes, it is called an empty tree.
If node doesn't have any children (it doesn't point to anything), it is called a leaf node.

A binary tree is a type of tree in which every node has at most two children nodes (left and right pointer).
Two nodes that descend form the same parent node are called sibling nodes.
A sub-tree is a term that can be used to describe any node that isn't the root node.
The node depth is how many levels of siblings a certain node is from the root node.
The root node has a depth of ``0``, it's children have a depth of ``1``, etc.

A full binary tree is a binary tree in which every non-leaf node has two children, and every leaf node has the same depth.

Traversals
----------

There are four different ways to traverse the nodes of a binary tree (to visit every node of a tree):
- pre-order,
- in-order,
- post-order, and
- level-order.

PRE-ORDER
.........

1. Do something with the current node (it could be to print it out, change the value, etc.)
2. Go to the nodes in the left sub-tree and repeat process
3. Go to the nodes in the right sub-tree and repeat process

Code:

.. code-block:: c++

   void preorder(Node *cur)
   {
        if (cur == nullptr)     // If the tree is an empty tree
             return;
   
        cout << cur->value;     // In this case we print out the value
   
        preorder(cur->left);     // Recursively do the same to the left and right sub-trees
        preorder(cur->right);
   }

.. code-block:: c++

           a
       b        c
   d       e

Example:

.. code-block:: c++

   Current root is a
   Print out a
   Go to left sub-tree
        Print out b
        Go to left sub-tree
             Print out d
             Go to left sub-tree
                  Return (no child)
             Go to right sub-tree
                  Return
        Go to right sub-tree
             Print out b
             Go to left sub-tree
                  Return
             Go to right sub-tree
                  Return
   Go to right sub-tree
        Print out c
        Go to left sub-tree
             Return
        Go to right sub-tree
             Return

Output:

.. code-block:: c++

   a b d e c

How to visualize it: Go down every line of nodes diagonally, top to bottom

IN-ORDER
........

1. Go to the nodes in the left sub-tree
2. Do something with the current node
3. Go to the nodes in the right sub-tree

Notice that in this order we first go all the way to the left before printing anything out (this includes the root node)

Code:

.. code-block:: c++

   void inorder(Node *cur)
   {
        if (cur == nullptr)
             return;
   
        inorder(cur->left);
   
        cout << cur->value;
   
        inorder(cur->right);
   }

.. code-block:: c++

           a
       b        c
   d       e

Example:

.. code-block:: c++

   Current root is a
   Go to left sub-tree (b)
        Go to left sub-tree (d)
             Go to left sub-tree (null)
                  Return
             Print out d
             Go to right sub-tree (null)
                  Return
        Print out b
        Go to right sub-tree(e)
             Go to left sub-tree (null)
                  Return
             Print out e
             Go to right sub-tree (null)
                  Return
   Print out a
   Go to right sub-tree (c)
        Go to left sub-tree (null)
             Return
        Print out c
        Go to right sub-tree (null)
             Return

Output:

.. code-block:: c++
   
   d b e a c

How to visualize it: start at the left side of tree, go vertically right, bottom to top

POST-ORDER
..........

1. Go to the nodes in the left sub-tree
2. Go to the nodes in the right sub-tree
3. Do something with the current node

Code:

.. code-block:: c++

   void postorder(Node *cur)
   {
        if (cur == nullptr)
             return;
   
        postorder(cur->left);
   
        postorder(cur->right);
   
        cout << cur->value;
   }

.. code-block:: c++

           a
       b        c
   d       e

Example:

.. code-block:: c++

   Current root is a
   Go to the left sub-tree (b)
        Go to the left sub-tree (d)
             Go to the left sub-tree (null)
                  Return
             Go to the right sub-tree (null)
                  Return
             Print out d
        Go to the right sub-tree(e)
             Go to the left sub-tree (null)
                  Return
             Go to the right sub-tree (null)
                  Return
             Print out e
        Print out b
   Go to the right sub-tree (c)
        Go to the left sub-tree (null)
            Return
        Go to the right sub-tree (null)
            Return
        Print out c
   Print out a

Output:

.. code-block:: c++

   d e b c a

How to visualize it: start at the bottom of the tree, go horizontally up, left to right

LEVEL-ORDER
...........

1. Use a pointer variable and a queue of pointers to nodes
2. Push the root node pointer onto the queue
3. While the queue is not empty,
     a. Pop the top node
     b. Do something to the node
     c. Push the node's children to the queue

Code:

.. code-block:: c++

   void levelorder(Node *cur)
   {
        if (cur == nullptr)
             return;
        Node *temp;
        queue<Node*> toProcess;
        toProcess.push_back(cur);
        while (!toProcess.empty())
        {
             temp = toProcess.front();
             toProcess.pop();
             cout << temp->value;
             if (temp->left != nullptr)
                  toProcess.push_back(temp->left);
             if (temp->right != nullptr)
                  toProcess.push_back(temp->right);
        }
   }

.. code-block:: c++

           a
       b        c
   d       e

Example:

.. code-block:: c++

   Current root is a
   Push a
   QUEUE: a
   Pop a, print out a
   QUEUE:
   Push b
   Push c
   QUEUE: b c
   Pop b, print out b
   QUEUE: c
   Push d
   Push e
   QUEUE: c d e
   Pop c, print out c
   QUEUE: d e
   Don't push anything
   Pop d, print out d
   QUEUE: e
   Don't push anything
   Pop e, print out e
   Don't push anything
   Done!

Output:

.. code-block:: c++

   a b c d e

How to visualize: Start from the top of the tree, go down horizontally, left to right


The big-O of all of these traversals is ``O(N)``.

You can also evaluate expressions with a binary tree.
For example, ``2 + 3`` would be represented with this tree:

.. code-block:: c++

      +
   2     3

Here is the algorithm to evaluate it:
1. If the current node's value is a number, return its value
2. Recursively evaluate the left sub-tree and get the result
3. Recursively evaluate the right sub-tree and get the result
4. Apply the operator in the current node to the left and right results; return the result.

.. code-block:: c++

   (5 + 6) * (3 - 1):

.. code-block:: c++

             *
       +         -
   5      6   3     1

Example:

.. code-block:: c++

   Current node is *
   Go to the left sub-tree
        Current node is +
        Go to left sub-tree
             Current node is 5
             Return 5
        Go to right sub-tree
             Current node is 6
             Return 6
        Apply + to 5 and 6
        Return 11
   Go to right sub-tree
        Current node is -
        Go to left sub-tree
             Current node is 3
             Return 3
        Go to right sub-tree
             Current node is 1
             Return 1
        Apply - to 3 and 1
        Return 2
   Apply * to 11 and 2
   Return 22