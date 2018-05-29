.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Algorithm Comparison
====================

Let us talk about how to compare algorithms.
Typically, the faster algorithm is better.
But how do we compare speed?
In computer science, you talk about the number of instructions an algorithm uses to for a certain size of data.
Different algorithms work better with different sizes of data, so you can't compare two algorithms without first specifying a variable data size.
Let's call the data size ``N``.
You would describe the number of instructions it takes in terms of ``N``; for example ``N^2`` or ``5N``.
By inputting a specific data size for ``N``, you would be able to compare the algorithms in the most accurate way.
This is the background of the **Big-O** concept; the ``f(n)``.
The Big-O concept is a way to compare algorithms for their worst case scenario by ignoring the coefficient (the ``5`` in ``5N``) and lower-order terms (the ``N`` in ``N^3 + N``).
Here some conversions from ``f(n)`` to ``Big-O``.

.. code-block:: c++

   N^2     O(N^2)
   4N^2   O(N^2)
   N^2 + 20   O(N^2)
   4N^2 + N   O(N^2)

.. code-block:: c++
   
   N*logN   O(N*logN)
   5N*logN  O(N*logN)
   N*logN + 20   O(N*logN)
   40N*logN + N   O(N*logN)


Sow how do you count how many instructions are performed?
Some examples of what an instruction is are:
- Accessing an item (like an item in an array)
- Evaluating a mathematical expression
- Traversing a single link in a linked list.

Let's first practice computing the ``f(n)`` of an algorithm.
Here is a really simple algorithm that prints out every item of an array.
Remember, we don't know how many items the array may have, so we just use ``n``.

.. code-block:: c++

   int arr[n];
   int i = 0;   // The initialization of a variable is equal to 1 step.
   // f(n) = 1
   while (i < n)  // It will compare i to n, n times.
   // f(n) = 1 + n
   {
      cout << arr[i] << endl;   // Accesses an item of the array n - 1 times
   // f(n) = 1 + n + n - 1
      i++;    // Increments i n - 1 times
   }

In total, ``f(n)`` is equal to ``2n``.
To convert it to a Big-O value, drop the coefficient and we get ``O(n)``.

If you had a two dimensional array, you would need two while loops.

.. code-block:: c++

   int arr[n][n];
   int i = 0;   // The initialization of a variable is equal to 1 step.
   // f(n) = 1
   while (i < n)  // It will compare i to n, n times.
   // f(n) = 1 + n
   {
        int j = 0;   // The initialization of a j happens n times.
        while (j < n)  // It will compare j to n, n^2 times (n time for j, n^2 times for j evey i)
        {
             cout << arr[i][j] << endl;   // Accesses an item of the array n^2 times
             j++;    // Increments j n^2 times
       }
   // f(n) = 1 + n + n + n^2 + n^2 + n^2
      i++;    // Increments i n times
   // f(n) = 1 + n + n + n^2 + n^2 + n^2 + n
   }

In total, ``f(n) = 3n^2 + 3n + 1``, or ``O(n^2)``.
When calculating the Big-O, instead of looking at every initialization, just look at the most frequently occurring operations.

Here are some hints to help you figure out the big-o.
- If you have multiple loops within one another, the big-o's exponent increases based on how many times each loop loops (in f(n) notation you would multiply the loops' n's).

.. code-block:: c++

   for (int i = 0, i < n; i++)
      ...  // O(n)

.. code-block:: c++

   for (int i = 0, i < n*n; i++)
      ... // O(n^2)

.. code-block:: c++

   for (int i = 0, i < n; i++) // n
      for (int j = 0, j < n; j++) // n
         ...
      ... // O(n^2) (f(n) = n * n)

.. code-block:: c++

   for (int i = 0, i < n; i++) // n
      for (int j = 0, j < n*n; j++) // n^2
         ...
      ... // O(n^3)

.. code-block:: c++

   for (int i = 0, i < n; i+=2)  // Notice that i is incremented by 2
      ... // O(n/2)

- If you have multiple loops that are NOT within one another, take the larger ``n`` (in ``f(n)`` notation you would add the loops' ``n``'s).

.. code-block:: c++

   void foo()
   {
   
   for (int i = 0, i < n; i++)  // n
      ...
   
   for (int i = 0, i < n*n; i++) // n^2
      ...
   } // O(n) (f(n) = n^2 + n)

- If you have a loop that that decreases how many times it loops exponentially by 2 (e.g. 8 times, 4 times, 2 times, 1 time), its big-o is ``log(n)`` (it is a base 2 ``log``).

.. code-block:: c++

   int i = n;
   while (i > 1)
   {
      ...
      i = i / 2;  // i divided in half every time
   }  // O(log(n))

.. code-block:: c++

   int i = n;
   while (i > 1)
   {
      ...
      i = i / 3;  // i divided in three every time
   }  // O(log3(n))

.. code-block:: c++

   int i = n * n; // Loops twice as many times as n
   while (i > 1)
   {
      ...
      i = i / 2;  // i divided in half every time
   }  // O(log(n^2))

Same multiplication rule as above applies here

.. code-block:: c++

   for (int j = 0; j < n; j++)  // n
   {
      int i = n;
      while (i > 1)
      {
        ...
       i = i / 2;  // i divided in half every time
      }
   }  // O(n*log(n))

.. code-block:: c++

   for (int j = 0; j < n*n; j++) // n^2
   {
      int i = n;
      while (i > 1)
      {
        ...
       i = i / 2;  // i divided in half every time
      }
   }  // O(n^2*log(n))

.. code-block:: c++

   void foo()
   {
      int i = n;
      while (i > 1)
      {
        ...
       i = i / 2;  // i divided in half every time
      }
   
      for (int i = 0, i < n; i++) // n
         ...
   
   }  // O(log(n)) (f(n) = log(n) + n) Note that n is bigger than log(n), so we keep it

.. code-block:: c++

   void foo()
   {
      int i = n;
      while (i > 1)
      {
         ...
         i = i / 2;  // i divided in half every time
      }
   
      for (int i = 0, i < n*n; i++)
         ... // O(n^2)
   
   }  // O(n^2) (f(n) = log(n) + n^2) Note that n^2 is bigger than log(n), so we keep it

.. code-block:: c++

   for (int j = 0; j < n; j++)
   {
      int i = n;
      while (i > 1)
      {
        ...
       i = i / 2;  // i divided in half every time
      }
   
      for (int i = 0, i < n; i++) // n
         ...
   }  // O(n*(log(n) + n)) = O(n^2 + n*(log(n)) 

You can also have the inside loop variables depend on outside variables, so they don't have a fixed number of iterations.
In this case you use the maximum number of iterations that loop can have.

.. code-block:: c++

   for (int i = 0; i < n; i++)     // n
     for (int j = 0; j < i; j++)   // Maximum iteration is when i is at its max (which is n), so at most j will run n times.
       ... // O(n^2) (n * n = n^2)

.. code-block:: c++

   for (int i = 0; i < n*n; i++)   // n^2
     for (int j = 0; j < i; j++)   // Maximum iteration is when i is at its max (which is n^2), so at most j will run n^2 times.
       ... // O(n^4) (n^2 * n^2 = n^4)

.. code-block:: c++

   for (int i = 0; i < n; i++)     // n
     for (int j = 0; j < i*i; j++) // Maximum iteration is when i is at its max (which is n), so at most j will run n^2 times.
       ... // O(n^3) (n * n^2 = n^3)

.. code-block:: c++

   for (int i = 0; i < n; i++)       // n
     for (int j = 0; j < i*i; j++)   // Maximum iteration is when i is at its max (which is n), so at most j will run n^2 times.
        for (int k = 0; k < j; k++)  // Maximum iteration is when j is at its max (which is n^2), so at most k will run n^2 times
           ... // O(n^5) (n * n^2 * n^2 = n^5)

.. code-block:: c++

   for (int i = 0; i < n; i++)       // n
     for (int j = 0; j < i*i; j++)   // Maximum iteration is when i is at its max (which is n), so at most j will run n^2 times.
        for (int k = 0; k < i; k++)  // Maximum iteration is when i is at its max (which is n), so at most k will run n times
           ... // O(n^4) (n * n^2 * n = n^4)

.. code-block:: c++

   for (int j = 0; j < n; j++)   // n
   {
      int i = j;
      while (i > 1)   // Maximum iteration is when j is at its max (which is n), so at most i will run log(n)
      {
         ...
         i = i / 2;   // i divided in half every time
      }
   } // O(n*log(n))

.. code-block:: c++

   for (int j = 0; j < n; j++)   // n
   {
      int i = j*j;     // At max i = n^2
      while (i > 1)    // Maximum iteration is when j is at its max (which is n), so at most i will run log(n^2)
      {
         ...
         i = i / 2;    // i divided in half every time
      }
   } // O(n*log(n^2))

.. code-block:: c++
   
   for (int j = 0; j < n; j++)   // n
   {
      Physicist arr[n];          // a
      arr[j].numComicBooks(j);   // b
   } // O(n^2) **Is this because of line a or b?

By the way, if you have something like

.. code-block:: c++

   for (int i = 0; i < z; i++)
        ...

you write the big-o in terms of ``z`` (``O(z)``).

You can also have two or more different and independent data sizes.
In that case you need to include ALL variables in your big-o calculation, even if one of them has a smaller coefficient.

.. code-block:: c++

   void foo(int a, int b)
   {
      for (int i = 0, i < a; i++)  // a
        ...
   
      for (int i = 0, i < b; i++)  // b
        ...
   } // O(a + b)

.. code-block:: c++

   void foobar(int a, int b)
   {
      for (int i = 0, i < a; i++)   // a
        ...
   
      for (int i = 0, i < b*b; i++) // b^2.
        ...
   } // O(a + b^2)

If you have a loop that runs depending on one data variables within another that depends on another, you ADD them, not multiply them.

.. code-block:: c++

   void bar(int a, int b)
   {
      for (int i = 0, i < a; i++)     // a
          for (int i = 0, i < b; i++) // b
             ...
   } // O(a + b)


.. code-block:: c++

   void barfoo(int a, int b)
   {
      for (int i = 0, i < a*a; i++)   // a
          for (int i = 0, i < b; i++) // b
             ...
   } // O(a^2 + b)

STL containers' operations also have big-o's.
Here is a run-down of all of them, assuming they have n items.

STACK and QUEUE
- Inserting an item     .push()     O(1)
- Popping an item       .pop()      O(1)
- Looking at the top    .top()      O(1)

VECTOR
- Inserting an item at the top or middle     []                        O(n)
- Inserting an item at the bottom            .push_back()              O(1)
- Deleting an item at the top or middle      .erase (w/ iterator)      O(n)
- Deleting an item at the bottom             .pop_back()               O(1)
- Accessing an item anywhere                 [], .front(), .back()     O(1)
- Finding an item                            w/ iterator               O(n)

LIST
- Inserting an item anywhere*                .push_front(), .push_back(), .insert()     O(1)
- Deleting an item anywhere*                 .erase()                                   O(1)
- Accessing an item at the top or bottom     .front(), .back()                          O(1)
- Accessing an item at the middle            w/ iterator                                O(n)
- Finding an item                            w/ iterator                                O(n)
* You may have to first iterate through X items (O(X))

SET
- Inserting an item     .insert()                O(log(n))
- Finding an item       .find()                  O(log(n))
- Deleting an item      .erase (w/ iterator)     O(log(n))

MAP
- Inserting a new item     [...] = ...     O(log(n))
- Finding an item          .find()         O(log(n))
- Deleting an item         .erase()        O(log(n))

Here is an example.

.. code-block:: c++

   set <int> nums;
   for (int i = 0; i < n; i++)     // n
        nums.insert(i);            // Inserting an item in a set, log(n)
   // O(n*log(n))

When evaluating these algorithms, assume that the container holds the maximum number of items possible, which is ``n`` (or whatever variable is being used).
So when you have ``.size()``, assume it is ``n``.

.. code-block:: c++

   vector <int> foo;
   while (foo.size() > 0)   // At max, foo.size() will be n
        foo.pop_back();     // Deleting an item from the end of a vector, O(1)
   // O(n)

.. code-block:: c++

   vector <int> bar;
   while (bar.size() > 0)        // At max, bar.size() will be n
        bar.erase(bar.begin());  // Deleting an item from the beginning of a vector, O(n)
   // O(n^2)

Here are some examples with a vector of integer sets.
To do more complex problems like these, start from the smaller/smallest container and go outwards

.. code-block:: c++

   vector<set <int> > foo;

Each vector has ``a`` sets
Each set has an average of ``b`` integers
Note: a set is automatically sorted alphanumerically.

What is the big-o of determining whether ``foo[0]`` contains the value 7?
- To find a value in a set, it is ``O(log(b))``
- To access an item of vector, it is ``O(1)``
- Answer: ``O(log(b))``

What is the big-o of determining whether any set contains the value 7?
- To find a value in a set, it is ``O(log(b))``
- To access an item of vector, it is ``O(1)``
- To access every item of a vector with a items, it is ``O(1*a) = O(a)``
- Answer: ``O(a * log(b))``

What is the big-o of determining the number of all the even values in all of foo?
- To access an item of a set, it is ``O(1)``.
- To access every item of a set with ``b`` items, it is ``O(1*b) = O(b)``.
- To access an item of vector, it is ``O(1)``.
- To access every item of a vector with a items, it is ``O(1*a) = O(a)``
- Answer: ``O(a * b)``

What is the big-o of finding the first set with a value of 7 and then counting the number of even numbers in that set?
- To find a value in a set, it is ``O(log(b))``
- To access an item of vector, it is ``O(1)``
- To access every item of a vector with a items, it is ``O(1*a) = O(a)``
- As a result, to determine whether any set contains the value 7, it is ``O(a * log(b))``
- To explore all the integers in the set containig the values 7, it is ``O(b)``.
- Answer: ``O(a * log(b) + b)``