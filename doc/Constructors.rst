.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

============
Constructors
============

In every class you need some way to initialize your variables.
You can make your own function that does this, but that means the user has to remember to call it every time they use the class.
That can lead to problems if the user forgets to because all simple variables start with a random value and this can lead to crazy results.
Luckily C++ has an initializing function that is automatically called whenever an object is created.
It is called a constructor.
C++ automatically creates a default constructor every time you use a class.
So why would you need to create your own?
The default constructor doesn't initialize any scalar member variables.
Scalar variables are anything that doesn't need it's own library or class definition: ints, doubles, bools.
Non-scalar variables are strings or any objects in a class you defined yourself.
Why doesn't it work with scalars? Scalars have random values until you initialize them, and the compiler leaves it up to you to decide what they should be initialized to.
However, if you create your own constructor, C++ will not create a default constructor.

You can have different constructors (aka overloading constructors) that take in a different number and/or type of parameters.
Most of the time the discrepancies are up to you, but if you have an array you must have a constructor that takes in no arguments.
Why?
Because the elements of an array can't have arguments.

When is the constructor called?

1) It is called whenever you create a new variable with a type of some class.

.. code-block:: c++

   Foo foo1;
   Foo foo1(5, "foo");   // note that two different constructors are called for these two

2) It is called for every element in an array (static and dynamic)

.. code-block:: c++

   Foo arr[42];        // Called 42 times

3) It is called when you declare a dynamically allocated variable.

.. code-block:: c++

   Foo *ptr = new Foo(6, "foofoo");

One case that it is NOT called is when you define a pointer variable of type class. Why? Because all pointers are the same in memory, and whatever the type of pointer may be, it has nothing to do with the actually composition of the type. It may look misleading because you see the name of class in front of it, but remember that it doesn't access the class directly.

.. code-block:: c++

   Foo *ptr;   // constructor NOT called

So what's the actually code for a constructor?

The name of a constructor must be the class name.
It can take no parameters or as many parameters as you like.
Just remember that if you define an array of your class type, you must have a constructor with no parameters!
The constructor has NO return type (like void, int, etc.) and cannot return anything (return 1, return 0, etc.).

When it's defined inside of the class (inline style) it looks like this (assuming the class is called Foo):

.. code-block:: c++

   Foo()
   {}

// or

.. code-block:: c++

   Foo(int bar, string str)
   {}

// etc.

If it's outside of the class you need to include the class name plus two colons in front of it:

.. code-block:: c++

   Foo::Foo()
