.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Unordered map
=============

The unordered map doesn't sort the items alphanumerically and uses a hash table.
That means its operations have the same big-O as those of a hash table.
You use it in the same way as a map, but you need to include

.. code-block:: c++

   #include <unordered_map>

instead of

.. code-block:: c++

   #include <map>

as well as

.. code-block:: c++

   using namepace std::tr1;

If you use a map that stores some self-defined class, you need to write your own hash function.
