.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Recursion
=========

Recursion is a type of algorithm used for functions that calls upon the function itself to solve a problem or implement something.
How does that even work?
Well, it's similar to having a loop, but every time the function calls itself, a new copy of the function is created in the stack.
When the last copy finishes (whenever or however that means) and returns,
the compiler will go back to the second to last copy, and when that returns, to the previous copy, all the way back to the original.
So unlike a loop, every iteration of a recursive function is saved with its own variables and values unique to that cycle.
Here is a simple example.
This is a function that calculates the power of some number.
For example, the user could write ``power(3, 7)`` and it would calculate what 3 to the power of 7 is.
Let's first create a function that does this with a for loop.

.. code-block:: c++

   int power(int num, int exp)
   {
        if (exp == 0)
             return 1;
        if (exp == 1)
             return num;
        int result = 1;
        // Use exp as a counter for how many times we need to multiple the number to itself,
        // so we don't need to declare anything before the first semicolon.
        for ( ; exp != 0; exp--)  
             result *= num;
        return result;
   }

When converting this into a recursive function, we will be rewriting the while loop so that it does the same thing but with recursion.
We want to keep the first five lines of code.
The two if statements are what are called "base cases," meaning they check for the simplest cases that we don't need to do any work for. 

.. code-block:: c++

   int power(int num, int exp)
   {
        if (exp == 0)     // Base cases
             return 1;
        if (exp == 1)
             return num;
        int result = 0;
        ...
   }

Now let's take the content of the for loop and put it after the base cases.


.. code-block:: c++

   int power(int num, int exp)
   {
        if (exp == 0)     // Base cases
             return 1;
        if (exp == 1)
             return num;
        int result = 1;
        result *= num;
        ...
   }

And now let's add our recursive statement, that is a call to the function itself.
What do we put as the parameters?
We want to pass on the same number to the next iteration, so that stays the same, but we need to reflect the while loop, which decrements exp every iteration.
We want to pass into our function ``exp - 1``.

.. code-block:: c++

   int power(int num, int exp)
   {
        if (exp == 0)     // Base cases
             return 1;
        if (exp == 1)
             return num;
        int result = 1;
        result *= num;
        power(num, exp - 1);
        ...
   }

The basic principle of recursive functions is that you want to keep on reducing the size or amount of information processed until you reach your base cases.
What you do with the info from the base class depends on the problem.
In our case, we want to keep on reducing the quantity of exp until it equals 1, and then it will return the number itself.
All of the iterations together should return the number exp times, so if expression is ``2`` to 4th power, ``2`` will be returned 4 times.
What do we do with four number ``2``s? We want to multiply all of them. 
So instead of just writing ``result += num * num``, let's combine that with the recursive statement below and add a return statement.

.. code-block:: c++

   int power(int num, int exp)
   {
        if (exp == 0)     // Base cases
             return 1;
        if (exp == 1)
             return num;
        int result = 1;
        result = num * power(num, exp - 1);
        return result;
   }

Notice that we change the ``*=`` to ``=`` because the expression ``esult = num * power(num, exp - 1);`` will accumulate all the multiplicants by recursion.
Result will finally be calculated once for every iteration and every time
(except for the last time, when the function returns for the last time and it ends)
``power(num, exp - 1)`` is executed it will replace itself with the return result.
You can think of it as 

.. code-block:: c++

   result = num * num * ... ;

where num is multiplied by itself ``exp`` times.
The iteration will keep on going deeper and deeper until result is finally calculated,
which is when ``power(num, exp - 1)`` returns ``1`` and result equals ``num * 1``.

You can also write the function so that you just directly return the entire statement, without the "result" variable middle man.

.. code-block:: c++

   int power(int num, int exp)
   {
        if (exp == 0)
             return 1;
        if (exp == 1)
             return num;
        return num * power(num, exp - 1);
   }