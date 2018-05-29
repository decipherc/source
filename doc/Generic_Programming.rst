.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Generic programming
===================

Generic programming is a technique that allows you to build one algorithm that can be used with different types of data.
You can make generic functions and generic classes with templates.
You can make generic comparisons by creating custom comparison operators.
Just like the assignment operator function overrides the assignment operator ``=``,
you can do the same with the less than or equal, less than, greater than or equal, and greater than.
Let's say we want to compare Physicists by their IQs. Instead of writing

.. code-block:: c++

   if (sheldon.iq() > raj.iq())
        return true;
   return false;

every time we want to compare them, we want to just be able to write

.. code-block:: c++

   if (sheldon > raj)
       .....

and it would automatically compare each object's IQs.
To this we need to create a function called ``operator>`` (or what ever operator you want) that takes in two const references to objects.
We can just use the code from above as the body.

.. code-block:: c++

   bool operator>(const Physicist &a, const Physicist &b)
   {
        if (sheldon.iq() > raj.iq())
              return true;
        return false;
   }
