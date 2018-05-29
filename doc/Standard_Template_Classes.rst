.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Standard Template Library (STL) Classes
=======================================

Just like queue and stack are pre-written functions in the Standard Template Library (STL), there are more.
We will be looking at the vector class, list class, map class, and set class.

Vectors
-------

A vector is basically like a dynamically allocated array.
You add (push) and remove (pop) items in a similar way to a queue, but you can also access individual elements with brackets, like an array.

Here are some of the functions of the interface:
1) `push_back()``: The ability to add an item (put the value between the parentheses)
2) ``[]``: Access and/or change an item
3) ``pop_back()``: Remove the last item
4) ``front()``: Access the first item
5) ``back()``: Access the last item
6) ``size()``: Get the size of the vector
7) ``empty()``: Determine whether it is empty or not

To use vectors, include this at the top of the program.

.. code-block:: c++

   #include <vector>

To create a vector, write "vector", the type of the variables you want it to hold in angular brackets, the name you want it to have,
and in parentheses the number of items it starts out with.

.. code-block:: c++

   vector<double> foo(67);

You can also add a second parameter; this will initialize all the elements to that value.

.. code-block:: c++

   vector<string> bar(5, "poop");    // 5 items all initialized to "poop"

To access the nth element, use brackets.

.. code-block:: c++

   string second = bar[1];

You can *NOT* access elements outside of the existing ones.

.. code-block:: c++

   bar[6];   // No!

Lists
-----

A list is a class that creates a linked list and its nodes for you.
It has all of the same functions as the vector class, except that you can *NOT* use brackets to access individual elements.
A list also has the ability to add an item to the front and to add an item to the back, unlike a vector.

1) ``push_back()``: Add an item to the back of the list (put the value between the parentheses)
2) ``push_front()``: Add an item to the front (put the value between the parentheses)

To create a list, write ``list``, the type of the variables you want it to hold in angular brackets,
he name you want it to have, and in parentheses the number of items it starts out with.

.. code-block:: c++

   list<int> foo(44);

So how do you access individual elements?
For a linked list, you may think that you should use a pointer.
Well, with this class, you can't use one, you have to use an iterator variable, which is basically the same as a pointer,
but it is an object and only works for STL containers.
Specifically, you can use an iterator in correspondence with the list class' ``begin`` and ``end`` functions because they return an address.
The iterator knows what element it points to, as well as how to find the previous element and next element in a container,
so you can use the ``++`` with a linked list iterator and it will work fine.

To declare an iterator, write the class' name (``vector``, ``list``, etc.), its type in angular brackets, two colons, the word ``iterator``,
and the name you want it to have.

.. code-block:: c++

   list<float>:: iterator poo;    // iterator of type list
   vector<string>:: iterator par; // iterator of type vector

So how do ``begin()`` and ``end()`` work?
Set your iterator to the beginning.

.. code-block:: c++

   poo = foo.begin();    // poo points to the first element

To get the last element, set your iterator to end, which is one past the last element, and then move the iterator one back.

.. code-block:: c++

   poo = foo.end();
   poo--;     // Now poo points to the last element.

To cycle through all of the elements, just make a while loop, but be sure you stop before you go past the last element.
You can use the star operator to access the value of the element an iterator points to just like you can with pointer.

.. code-block:: c++

   poo = foo.begin();
   while (poo != foo.end())
   {
        cout << (*poo) << endl;    // Prints out the value of every element
        poo++;
   }

You can also use the arrow operator with an iterator.

Also, if you have a const list or vector (like in a parameter for a function), the iterator must also be const.
You declare a const iterator by writing "const" after the two colons and before "iterator."

.. code-block:: c++

   list<bool>::const iterator boo;

Maps
----

A map is used to associate two different values, which could be of any type.
It has five functions in its interface (from what we've learned).

1) ``[]``: Used ONLY for creating new map items
2) ``find()``: Returns the address of an item by putting its first parameter (only the first parameter will work) in between the parentheses. This should be used with an iterator so it can hold the address.
3) ``first``: When used with an iterator (``(*foo).first`` or ``foo->first``), gives the value of a certain item's first parameter.
4) ``second``: When used with an iterator (``(*foo).second`` or ``foo->second``), gives the value of a certain item's second parameter.
5) ``begin()``: Like a vector's and list's begin function, returns the address of the first item.
6) ``end()``: Like a vector's and list's end function, goes past the map's items.

To use a map, include this header.

.. code-block:: c++

   #include <map>

To declare a map, write "map", the two types of values you want to store in angular brackets and a comma in between, and the name of the map.

.. code-block:: c++

   map<int, string> foo;

To add a value, write the name of the map, the value of the first type in between brackets, and set it equal to the second value.

.. code-block:: c++

   foo[1] = "uno";

In the map we declared, we can only associate an int to a string, not a string to an int.

.. code-block:: c++

   foo["dos"] = 2;    // No!

If you want to associate a string to an int, you would have to create a separate map that is declared with the string as the first parameter.

.. code-block:: c++

   map<string, int> bar;

Note that a map sorts all of the items by alphabetical/numerical order (depends on what the first parameter is).
Because of this, if you have a self-defined struct or class as the first parameter (not necessary if it is the second parameter),
you need to create your own less than comparison operator so that map can sort the elements.

.. code-block:: c++

   class Foo
   {
   public:
      string get();
   private:
      string m_foo;
   };

.. code-block:: c++

   bool operator<(const Foo &a, const Foo &b)
   {
       return (a.get() < b.get());
   }

Sets
----

A set is is similar to an array; it holds items of one type, but they are also alphabetically ordered and you can access an item by its value quickly.

A set has four interface functions, plus a ``begin()`` and ``end()`` function that work the same as in the order classes we have seen.
1) ``insert()``: Adds an item to the set; the value goes in the parentheses.
2) ``size()``: Returns the size of the set.
3) ``erase()``: Erases an item that has the value that's in the parentheses. You can also have the iterator point to the item and delete the iterator. But that also means you can't use the iterator once you have used it to erase something.
4) ``find()``: Returns the address of an item based on its value that is in the parentheses. If it doesn't find it, it will return the address of ``end()``. Use an iterator to hold the address

To use the class include this:

.. code-block:: c++

   #include <set>

To create a set, write "set", the type of value in between angular brackets, and the name you want it to have.

.. code-block:: c++

   set<int> foo;

Just like a map, the set organizes the items by alphabetical/numerical order.
So, like with a map, if you create a set of structs or classes, you must define the less than comparison operator for it.

Note for vectors and iterators: If use an iterator and point to some where in the vector, and then it you either add or erase an item from the vector, the iterator will NOT work any more, regardless of whether where that item is in relation to the iterator.
This is because a vector changes how much memory it has open depending on the number of items, so it could change it without you knowing, and then the iterator may not point to the same place.
