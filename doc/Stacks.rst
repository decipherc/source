.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

======
Stacks
======

The first is a stack.
Let's start by talking about the algorithm.
The idea is that the first item in is the last out (LIFO), or the last item in is the first out.
Think of it like a stack of plates.
The last one you place in will be the first one to use.
In the example below, 3 is the last one to be put in and the first to be taken out.

.. code-block:: c++

   3
   -
   2
   -
   1

Why would this algorithm be useful? It can be used for evaluating mathematical expressions and converting infix expressions to postfix expressions (A + B -> A B +).
It is also used to store actions done in say, a word processing program, which is how the undo button works.
Say I write "My name is Netta".
The stack would look like this as you type it in:

.. code-block:: c++

   "My"
   ----

Then:

.. code-block:: c++

   "name"
   ------
   "My"
   ----

Then:

.. code-block:: c++

   "is"
   ----
   "name"
   ------
   "My"
   ----

Then:

.. code-block:: c++

   "Netta"
   -------
   "is"
   ----
   "name"
   ------
   "My"
   ----

To undo the last thing we wrote, the program would remove the last item, leaving us with "My name is":

.. code-block:: c++

   "is"
   ----
   "name"
   ------
   "My"
   ----

Then you can fix the mistake:

.. code-block:: c++

   "Neda"
   ------
   "is"
   ----
   "name"
   ------
   "My"
   ----

Now let's look at the data structure of a stack.
The most common one is also called a stack, and it's defined in the Standard Template Library (STL).
You could also implement your own data structure, like an array or linked list, and use the same algorithm with it.
To use a stack from the STL include this in your code:

.. code-block:: c++

   #include <stack>

Or you can write ``"std::"`` in front of any functions or keywords you use.
To create a stack, write ``"stack"``, the name of the type of value you want it to hold in angular brackets
(I believe this could also be an object), and the name you want it to have.

.. code-block:: c++
   
   stack<string> foo;

The advantage to using the stack defined in the STL is that it includes predefined functions for the stack interface.
If you used other data structures, you would have to define these functions. 
The interface of a stack consists of five basic functions.

1. Put something on the top of the stack.
Note that the top is the only place where you can insert an item in a stack.
This function is called "push" and its parameter(s?) is the value you want to insert.
It doesn't return any value.

2. Remove the top item. Note that the top is the only place where you can remove an item in a stack.
This function is called "pop" and it takes no parameters (why? It automatically takes off the top, no need for anything else).
Also note that this function does NOT return any thing, so it will NOT tell you what value it has just removed.
If you want to know what value you removed, you need to use this next function.

3. See the value of the top item.
This function is called "top" and it takes no parameters.
It returns the value of the top item, and does nothing else.

4. See whether the stack is empty. This function is called "empty" and it takes no parameters.
It returns a boolean value; true if it's empty, false if it isn't.

5. Gets the size of the stack.
This function is called "size" 

Let's see these functions in action!

.. code-block:: c++

   #include <iostream>
   #include <stack>
   using namespace std;
   
   int main()
   {
       stack<string> southPark;       // stack of strings
       southPark.push("Stan");       // Adds "Stan" to the stack
       southPark.push("Kyle");
       southPark.push("Cartman");
       southPark.push("Kenny");
   
       cout << "They killed " << southPark.top() << " !" << endl;
       cout << "You bastards!" << endl;
       southPark.pop();
   
       if (!southPark.empty())
           cout << "And then there were " << southPark.size() << endl;
   }

Note that you need to call ``top()`` if you want to access the value before you use ``pop()`` on it.

Infix and Postfix expressions
=============================

Now let's look at a subject related to stacks: infix and postfix expressions.
These expressions can refer to any mathematical or logical expression.
In infix notation, the operator goes between the numbers it applies to.
Makes the most sense, right? Well, that's only because that's how we were taught math in school.
You can also write an expression in postfix expression, and the operators go after the number(s) it applies to.
Here are some expressions in infix and postfix notation:

.. code-block:: c++

   INFIX: 30 + 12
   POSTFIX: 30 12 +

.. code-block:: c++

   INFIX: (18 + 3) * 2 
   POSTFIX: 18 3 + 2 *

.. code-block:: c++

   INFIX: 18 + (3 * 2) 
   POSTFIX: 18 3 2 * +

So why the hell would we want to do it this way?
Isn't it just a more confusing way of writing?
Actually, it's less confusing! Let's look at two of the examples above. 

In examples 2 and 3, we differentiate between the two expressions in infix notation with parentheses.
But what about if the expression was just "18 + 3 * 2".
What is the correct interpretation of this expression?
Well, if you follow precedence rules, then it would be "18 + (3 * 2)".
But that means you have to know what the precedence rules are.
The postfix expression, however, is completely unambiguous: "18 3 2 * +".
Let me first explain how to read a postfix notation, and then you will understand.

Here is example 1 from above.
It's pretty self-explanatory, but let's still look at the proper way to read it.

Keep on reading the numbers until there's an operator.
Then you take the last two numbers and put the operator in between them and calculate the result.
The result REPLACES the two numbers and the operator.
Continue doing this until the expression is finished.
Now let's read this expression:

.. code-block:: c++

   30 12 +

We have a ``30``. We think "cool!" and keep on reading.

``12`` Think: "cool"

``+`` "Ooo something different."

Take the last two numbers (in the order they were read) and put the operator in between: ``30 + 12``.

The result is ``42`` ("Cool.").

Replace the two numbers and the operator with it.

``42`` 

Check: is there anything else?

Nope?

That's it! 

Now let's do the Example 2.

.. code-block:: c++
   
   18 3 + 2 *

``18`` "Cool."

``3`` "Cool."

``+``  Now we do something!

Take the last two numbers and put the operator in between and calculate: ``18 + 3 =  21``. 

Replace the numbers and the operator with the result.

``21 2 *``

Let's keep going!

``21`` "Cool."

``2`` "Cool."

``*`` Yay! ``21 * 2 = 42`` ("Cool.").

``42``

Is there anything else?
Nope, we are done!
Notice that this is indeed the same as doing ``(18 + 3) * 2``: ``18 + 3`` is ``21``, ``21 * 2`` is ``42``.

And finally, example 3.

.. code-block:: c++
   
   18 3 2 * +

Notice that now we have three numbers and then an operator.
Wait!
No! 
Don't panic!
Let's just go through our steps and you'll see that it all works out.

``18`` "Cool."

``3`` "Cool."

``2`` "Cool."

``*``  Oh no! 
What do we do? 
You take the last TWO numbers and apply the operator to them. 
``3 * 2 = 6`` ("Cool.").
Replace it and you get:

.. code-block:: c++
 
   18 6 +

Whew!
Now it's not so bad.
Just keep on reading from where you left off.

``+`` Last two numbers were: ``18`` and ``6``. ``18 + 6 = 24`` ("Cool.").

``24``

Notice that this is the same as doing ``18 + (3 * 2)``: ``3 * 2`` is ``6``, ``18 + 6`` is ``24``.

How can we get a computer to do this? By using a stack!
Let's translate the way we read a postfix expression into our stack's interface functions.
Instead of saying "Cool." every time we have a new token (a number or operator), we will push the token onto the stack.
Every time we read a token that is an operator, we pop off the last two items of the stack.
After we do our calculation and say "Cool.", we will now push it onto the stack (and say "Cool." in our heads).
Let's translate the instructions for example 3 into pseudo code.

.. code-block:: c++
   18 3 2 * +
   18: Push onto stack.
   3: Push onto stack.
   2: Push onto stack.
   *: Pop off last item. Pop off last item. Apply the operator to them. 3 * 2 = 6. 
   6: Push onto stack.
   18 6:
   +: Pop off last item. Pop off last item. Apply the operator to them. 18 + 6 = 24. 
   24: Push onto stack.
   24

Now we can make this into actual code (I'm going to assume that the user is inputting valid tokens).

.. code-block:: c++
   #include <iostream>
   #include <stack>
   using namespace std;
   
   int main()
   {
        stack<int> infix;
        int num1 = 0;
        int num2 = 0; 
        int result = 0;
       cout << "Enter your infix expression; press enter after every character: ";
       while (cin != 'q')     // Is this legal? I don't know how else to do this 
       {
             cin >> char i;
             if (checkIfDigit(i))   // Not going to write this function, but we need something like this because isdigit() doesn't work on char
             {
                  i = convertToInt(i); // Not going to write this function, but you get the idea. You have to convert the char to int for it to work
                  push(i);
             else
             {
                  num1 = top();
                  pop();
                  num2 = top();
                  pop();
                  result = calculateResult(num1, num2, i);  // Not going to write this function, but you get the idea. It would have to translate the char into the right mathematical operator
                  push(result);
             }
        }
   }