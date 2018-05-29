.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Copy Constructors and Assignment Operators
==========================================

Here is a little more on the syntax of copy constructors and assignment operators that I have not gone over.

A copy constructor's job is to create a new variable based on the content of an existing one.
It does this by
(1) allocating memory for the new variable based on how much memory is allocated in the old one and
(2) copying the data from the old variable into the new one.
Unlike the assignment operator, the copy constructor does NOT deallocate memory.
Why?
There is nothing for it to deallocate.
You are creating a new variable from scratch, and you certainly don't want to destroy the existing variable.

The name of the copy constructor must be the class name.
So how do you differentiate it from the constructor, whose name is also the class name?
The copy constructor takes in a constant reference of type class.
It also has no return type.
Why? It's like a constructor.
They both don't have return types because they aren't technically called by the code, they are called by the memory allocation, which implicitly calls them.
This has to do with the fact that whenever you create a new variable, the compiler has to figure out how much memory to allocate first, and then actually construct it.

.. code-block:: c++

   class Foo
   {
   ..........................
   Foo (const Foo &src) {...}
   ..........................
   };

// OR

.. code-block:: c++

   class Foo {...};
   Foo::Foo (const Foo &src) {...}

An assignment operator's job is to change the contents of an existing variable (target variable) based on the contents of another variable (source variable).
It does this by
(1) checking that it is not attempting to change the contents of two variables that refer to the same variable (aliasing);
(2) deallocating all memory in the target variable (deleting it);
(3) copying the data from the source variable into the target; and
(4) returning a reference to the target.
Why do we have to do the first step?
If you don't prevent it, the code will go on to delete the target variable (which is also the source variable),
and then attempt to copy the contents of the source variable into the target variable, but now neither of them exist.
The check is done by this code which should be put at the beginning of the code:

.. code-block:: c++

   if (&src == this)
      return (*this);

The name of assignment operator must be ``operator=``.
It must return a reference to the class.
Like the copy constructor, it must take in a constant reference to the class as a parameter.
Unlike the copy constructor, it does have return type.
Why?
As we said before, the copy constructor is called through the memory allocation because you're creating a new variable.
But the assignment operator does NOT create a new variable, so it doesn't go through memory allocation, so it IS called by the code.
How are you calling it?
See where it happens:

.. code-block:: c++

   int bar(5);
   int boo;
   boo = bar;    // By using "=" to assign an existing variable to another, you are in fact calling the assignment operator.

Here is the syntax for the function definition

.. code-block:: c++

   class Bar
   {
   ....................................
   Bar& operator=(const Bar &src) {...}
   ....................................
   };

// OR

.. code-block:: c++

   class  Bar {...};
    Bar& Bar::operator=(const  Bar &src) {...}

You need to define your own copy constructor/assignment operator/destructor in the case of dynamically allocated variables and arrays.
The implicit copy constructor/assignment operator/destructor works fine for ALL static variables.
It's the dynamic array that needs a deep copy.

Why do you need to define your own copy constructor in the case of a dynamic variable?
The compiler-generated copy constructor will do a shallow copy, meaning it will just copy the values,
but it WON'T create a new dynamic variable, so you'll end up with two variables that really just two different names for the same variable in memory (alias).

Why do you need to define your own assignment operator in the case of a dynamic variable?
The compiler-generated assignment operator WON'T deallocate the memory and instead just do a shallow copy (also known as a memberwise copy).

Why do you need to define your own destructor in the case of a dynamic variable?
The compiler doesn't destroy variables in the heap with the implicit destructor.
