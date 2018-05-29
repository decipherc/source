.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Tables
======

A table is a type of ADT that holds records. A record is a group of related data.
In terms of data structures, a record would be a struct, and you would hold the structs in an array or vector.


For example, say you have a struct with a student's name, GPA, and ID.

.. code-block:: c++

   struct Student
   {
        string name;
        double gpa;
        int id;
   };

A key field is a field that has unique values across all records.

.. code-block:: c++

   struct Student
   {
        string name;
        double gpa;     // not key field
        int id;     // key field
   };
   
An efficient table also has indexes that associate one field to another so that you can search for one field with another (you often search with key fields).
These indexes can be maps, binary search trees, or hash tables.
It depends on whether you want your data to be sorted alphanumerically or not (if so, use BST, otherwise you a hash table).

One thing to note is that every time you update or delete a record, you need to change it across all indexes.