.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Heap
====

A heap is a type of data structure that orders its items either by largest to smallest or vice versa.
It uses a complete binary tree.
A complete binary tree is a binary tree whose top ``n-1`` levels are completely filled, and the nodes on the bottom-most level must be as far left as possible (meaning there are no empty children pointers in between).

Examples of valid complete binary trees (x represents empty child pointer):

.. code-block:: c++

         a
     b       c
   d   e    x  x

.. code-block:: c++

         a
     b        c
   d   e    f   x

.. code-block:: c++

         a
     b       c
   d   e    f  e

.. code-block:: c++

        a
     b    x

Examples of invalid complete binary trees:

.. code-block:: c++
         a
     b       c
   d  x     e  x

Notice that there is an empty pointer in between d and e

.. code-block:: c++

         a
     b      x
   d  e

There are two types of heaps: maxheap and minheap.
Maxheap organizes its items with the largest item at top and minheap has the smallest item at top.
Note that a heap is NOT a binary search tree.
A maxheap follows these rules: the value of any node must be greater than or equal to the values of its two children.
In a minheap, the value must be less than or equal to the values of the children.
Notice that in a BST, the left subtree must be less than any node's value and right subtree must be greater.
In a heap the value must greater than or less than both of its children.
In a maxheap, the biggest item is always the root of the tree. In a minheap, the smallest item is the root of the tree.

Let's go over the operations

Extracting the biggest item from a maxheap (smallest item from a minheap)
-------------------------------------------------------------------------

Algorithm:
1. If tree is empty, don't do anything.
2. Otherwise, save the top item as the biggest/smallest item
3. If the heap only has one node, return the value of that node and delete the node
4. Otherwise, copy the value from the bottom-most, right-most node into the root node.
5. Delete that node
6. Repeatedly swap the just-moved value with the larger/smaller of its two children until all the nodes are greater than or equal to both of their children.
7. Return the saved value

Notice that you can't just return the biggest/smallest item and return; you must find a replacement to be the next biggest/smallest item.

Example with maxheap:

.. code-block:: c++

         7
      2     5
   1

Save ``7`` as the biggest value

Find the bottom-most, right-most value: ``1``

Copy it into the top node and delete the node it was in before

.. code-block:: c++

        1
     2     5

Compare ``1`` with its children: ``2`` and ``5``. ``5`` is the larger of the two children and is also larger than ``1``, so swap ``1`` and ``5``

.. code-block:: c++

        5
     2     1

We are done!

Example with minheap:

.. code-block:: c++


         1
      2     5
   7

Save ``1`` as the smallest value

Find the bottom-most, right-most value: ``7``

Copy it into the top node and delete the node it was in before

.. code-block:: c++

        7
     2     5

Compare ``7`` with its children: ``2`` and ``5``. ``2`` is the smaller of the two children and is also smaller than ``7``, so swap ``7`` and ``2``

.. code-block:: c++

        2
     7     5

We are done!

Adding a node to a maxheap/minheap (reheapification)
----------------------------------------------------

Algorithm:
1. If the tree is empty, create a new root node, copy the value, and return
2. Otherwise, insert the new node in the left-most, bottom-most position of the tree (so we create a complete tree) and copy the value
3. Repeatedly compare and swap the value with its parents until all the values are in the correct place

Example with maxheap, inserting ``9``:

.. code-block:: c++

         7
      2     5
   1

Create a new node in the left-most, bottom-most position of the tree and copy 9 into it

.. code-block:: c++

        7
     2     5
   1   9

Compare ``2`` and ``9``. ``2`` is smaller than ``9``, so we swap

.. code-block:: c++

        7
     9     5
   1   2

Compare ``9`` and ``7``. ``7`` is smaller than ``9``, so we swap

.. code-block:: c++

        9
     7     5
   1   2

Compare ``9`` and ``5``. ``9`` is greater than ``5``, so we are done

Example with minheap, inserting 2:

.. code-block:: c++

         1
      3     5
   7

Create a new node in the left-most, bottom-most position of the tree and copy ``2`` into it

.. code-block:: c++

        1
     3     5
   7   2

Compare ``2`` and ``3``. ``3`` is greater than ``2``, so we swap

.. code-block:: c++

        1
     2     5
   7   3

Compare ``2`` and ``1``. ``1`` is smaller than ``2``, so we are done

So we have gone over the algorithm and the interface, now what about the data structure?
Maybe it seems obvious to use a binary tree with node structures, but it actually gets kind of complicated to find the bottom-most, right-most/left-most node and finding the node's parent is not easy (for swapping).
So instead, we could use an array.
Just copy every level, left to right, (a la level-order traversal) into the array, with the root value at ``arr[0]``.
You also need to keep a count of the number of values.

Example:

.. code-block:: c++

        7
     2     5
   1

.. code-block:: c++

   int arr[100] = { 7, 2, 5, 1 }
   int count = 4;

This is actually much more efficient! Here are some of the properties:
- The root value is always at ``arr[0]``
- The bottom-most, right-most node is always in ``arr[count - 1]`` (for extracting an item)
- The bottom-most, left-most empty spot is always in ``arr[count]`` (for adding an item)
- To find a the left child of a parent by using slot numbers, simply do ``(2 * index) + 1`` where index is the number of the array slot of a certain node.
- To find a the right child of a parent by using slot numbers, simply do ``(2 * index) + 2``
- To find the parent's slot of any given node, simply do ``(index - 1) / 2``

Example:
.. code-block:: c++

     0  1  2  3
   { 7, 2, 5, 1 }

Let's try finding the slot of the parent node of the node at ``3`` (value ``1``).

.. code-block:: c++

   (index - 1) / 2
   (3 - 1) / 2 = 1

The item at slot ``1``, value ``2``, is the parent.
If we check back to the tree, we see that this is indeed correct

Finding the left child of the item at slot ``0``, item ``7``:

.. code-block:: c++

   (2 * index) + 1
   (2 * 0) + 1 = 1 // Value 2

Finding the right child of the item at slot ``0``, item ``7``:

.. code-block:: c++

   (2 * index) + 2
   (2 * 0) + 2 = 2 // Value 5

Extracting from a maxheap/minheap (the array version):

1. If it is an empty tree (``count == 0``), return
2. Otherwise, save the value of ``arr[0]`` (it is the biggest/smallest value)
3. If ``count == 1`` (there is only one node), then set the ``count`` to ``0`` and return the saved value
4. Copy the value of the bottom-most, right-most into the root node with ``arr[0]`` (``arr[0] = arr[count - 1]``)
5. Delete the right-most node in the bottom-most row with ``count--``
6. Repeatedly compare and swap the values of the just-moved value and the larger/smaller of its children. Starting with ``i = 0``, compare and swap ``arr[0]`` with ``arr[2 * i + 1]`` and ``arr[2 * i + 2]``
7. Return the saved value

Adding a node to a maxheap/minheap (the array version):

1. Insert a new node in the left-most, bottom-most open slot with ``arr[count] = value``; ``count++``;
2. Compare the new value (``arr[i]``) with its parent value ``arr[(i - 1) / 2]``. If the parent is bigger/smaller, swap them
3. Repeatedly swap the values of the tree until it is correct

The big-O of extracting the max/min item from a max/minheap with n values is ``O(log(n))`` because you need to reposition the values.
The worst case is ``log(n)``.
The big-o of inserting a new item into a heap is ``O(log(n))`` because you may need to swap the new value with its parent, etc.
The worst case is ``log(n)``.
