.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

======
Queues
======

A queue is another kind of ADT.
In case you didn't know, a queue is what they call a line, like a line at the store, in British English.

Algorithm
=========

The elements of a queue (in C++) are ordered like a line.
The first person in line is the first person out (FIFO).
A queue is a counterpart to the stack.
In the example below, 3 is the last item to be added and the last one out.

.. code-block:: c++

   front -> 1 | 2 | 3

You can only add items to the rear of the stack, and you can only take away items from the front.
Adding items is called enqueuing and taking away items is dequeuing.

Data structures
===============

Just like stacks, queues are in the STL.
You need to include this to use it fully (that is, without writing "std::" in front of every queue keyword).

.. code-block:: c++

   #include <queue>

To create a queue, write "queue", the type of item you want it to hold in angular brackets, and then the name you want.

.. code-block:: c++

   queue<string> foo;

You can also use an array or linked list as a data structure for a queue ADT.

Interface
=========

Just like the stack, the queue has slightly different functions (as the algorithm of the queue itself is different) with almost the same names.

1. Put something at the rear of the queue.
Note that the rear is the only place where you can insert an item in a queue.
This function is called "push" and its parameter(s?) is the value you want to insert.
It doesn't return any value.

2. Remove the first item.
Note that the top is the only place where you can remove an item in a queue.
This function is called "pop" and it takes no parameters.
Also note that this function does NOT return any thing, so it will NOT tell you what value it has just removed.
If you want to know what value you removed, you need to use this next function.

3. See the value of the first item.
This function is called "front" and it takes no parameters 
(note that this has a different name than the stack).
It returns the value of the first item, and does nothing else.

4. See whether the queue is empty.
This function is called "empty" and it takes no parameters.
It returns a boolean value; true if it's empty, false if it isn't.

5. Gets the size of the queue.
This function is called "size".

Here is the same program from the stack email, but rewritten to work with a queue. 

.. code-block:: c++

   #include <iostream>
   #include <queue>
   using namespace std;
   
   int main()
   {
       queue<string> southPark;       // queue of strings
       southPark.push("Kenny");   // Now this element must be pushed first
       southPark.push("Stan");       // Adds "Stan" to the queue 
       southPark.push("Kyle");
       southPark.push("Cartman");
   
       cout << "They killed " << southPark.front() << " !" << endl;
       cout << "You bastards!" << endl;
       southPark.pop();
   
       if (!southPark.empty())
           cout << "And then there were " << southPark.size() << endl;
   }
   
