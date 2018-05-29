.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Heapsort
========

The heapsort is a type of sort that uses a maxheap.
It has two main steps:
1. Convert an input array into a maxheap,
2. Remove the biggest value from the heap and put it at the end.

Let's go over the Step 1 first

Step 1
------

Algorithm:

1. Set ``curNode`` to the ``(n / 2) - 1`` (this way we skip all the leaf nodes that by default are valid maxheaps)
2. Repeatedly decrease ``curNode`` until ``curNode`` is equal to the ``root`` node
     a. Think of the subtree pointed to by ``curNode`` as a maxheap
     b. Keep shifting the top value until that subtree becomes a valid maxheap

By the end of Step 1, the inputted array will be sorted as a maxheap

Step 2
------

Algorithm:

1. Extract the biggest value from the maxheap (root node).
2. Copy the value from the bottom-most right-most node and put it in the root
3. Delete that node
4. Repeatedly swap the just-moved value with the larger of its two children until the value is greater than or equal to both of its children
5. Put the extracted value into the freed-up slot

Example:

.. code-block:: c++

   arr[8] = { 0, 3, 9, 1, 5, 4, 18, 21};

Even though we will be using an array as the data structure, you should draw a tree to visualize the data easier.

.. code-block:: c++

              0
         3         9
     1     5    4   18
   21

``curNode = (8 / 2) - 1 = 3``.
Slot 3 has the value ``1``

.. code-block:: c++

              0
         3         9
     1     5    4   18
   21

Think of ``curNode`` and its children as a subtree.
Is it a valid maxheap?

.. code-block:: c++

     1
   21

.. code-block:: c++

No, we need to swap them

.. code-block:: c++
     21
   1

Now it is valid.

**Decrease curNod**e.
Now it points to the value at slot 2, ``9``.
Is that subtree a valid maxheap?

.. code-block:: c++

                  9
                4   18

No, swap ``9`` and ``18``

.. code-block:: c++

                  18
                4   9

Now it is valid.
**Decrease curNode**.
Now it points to the value at slot 1, ``3``.
Is that subtree a valid maxheap?

.. code-block:: c++

         3
     21     5
   1

No, swap ``3`` and ``21``

.. code-block:: c++

        21
     3      5
   1

Now it is valid.
**Decrease curNode**.
Now it points to the value at slot 0, ``0``.
Is that subtree (now entire tree) a valid maxheap?

.. code-block:: c++

              0
        21        18
     3     5    4    9
   1

No, swap ``0`` and ``21``

.. code-block:: c++

             21
         0       18
     3     5   4    9
   1

Now we need to swap ``5`` and ``0`` as well

.. code-block:: c++

             21
        5        18
     3     0   4    9
   1

Now it is valid.

Here is what the array look like now:

.. code-block:: c++

   arr[8] = { 21, 5, 18, 3, 0, 4, 9, 1 }

Extract the biggest value from the maxheap and copy the value from the bottom-most right-most node into the node

.. code-block:: c++

             1
        5        18
     3     0   4    9

.. code-block:: c++

   { 1, 5, 18, 3, 0, 4, 9,  }

Make all the necessary swaps to make it a maxheap

.. code-block:: c++

            18
        5        1
     3     0   4    9

.. code-block:: c++

            18
        5        9
     3     0   4    1

.. code-block:: c++

   { 18, 5, 9, 3, 0, 4, 1,  }

Now it is valid.
We insert the ``21`` back into the array at the freed-up slot.
We ignore this newly inserted item when drawing out our tree, however.

.. code-block:: c++

   { 18, 5, 9, 3, 0, 4, 1, 21 }

.. code-block:: c++

            18
        5        9
     3     0   4    1

Extract ``18`` and replace it with the bottom-most right-most node and do all necessary swaps

.. code-block:: c++

            1
        5        9
     3     0   4

.. code-block:: c++

            9
        5        1
     3    0    4

.. code-block:: c++

            9
        5        4
     3    0    1

Insert ``18`` into the freed-up slot

.. code-block:: c++
   
   { 9, 5, 4, 3, 0, 1, 18, 21 }

Extract ``9``, etc.

.. code-block:: c++

            1
        5        4
     3    0

.. code-block:: c++

            5
        1        4
     3    0

.. code-block:: c++

            5
        3        4
     1    0

.. code-block:: c++

   { 5, 3, 4, 1, 0, 9, 18, 21 }

Extract ``5``, etc.

.. code-block:: c++
   
            0
        3        4
     1
     
.. code-block:: c++

            4
        3        0
     1

.. code-block:: c++

   { 4, 3, 0, 1, 5, 9, 18, 21 }

Extract ``4``, etc.

.. code-block:: c++

            1
        3        0

.. code-block:: c++

            3
        1        0

.. code-block:: c++

   { 3, 1, 0, 4, 5, 9, 18, 21 }

Extract ``3``, etc.

.. code-block:: c++

            3
        1        0

.. code-block:: c++

            0
        1

.. code-block:: c++

            1
        0

.. code-block:: c++

   { 1, 0, 3, 4, 5, 9, 18, 21 }

Extract ``1``, etc.

.. code-block:: c++

        0

.. code-block:: c++

   { 0, 1, 3, 4, 5, 9, 18, 21 }

Extract ``0``

.. code-block:: c++

   { 0, 1, 3, 4, 5, 9, 18, 21 }

The big-o of the heapsort is ``O(n * log(n))`` for ``n`` items
- The big-o of Step 1 is ``O(n)``
- The big-o of Step 2 is ``O(n * log(n))`` (every time you remove an item it takes ``log(n)`` steps to reposition the array, and you remove n items)
- The total is ``O(n + n * log(n))``, which simplifies to ``O(n * log(n))``