.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Priority queue
==============

A priority queue is an algorithm that sorts its items by priority rating.
When you dequeue an item, it gives you the item with the highest priority rating.
Here are its three operations of the interface:

1. Insert an item into the queue
2. Get the value of the highest priority item
3. Remove the highest priority item from the queue

You need to define how priority is calculated.
A priority queue uses a heap as a data structure.
