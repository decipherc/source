.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

===========
Destructors
===========

Destructors delete all variables you may have defined in your class.
Just like with constructors, C++ will define a destructor for you if you don't.
Then why do you need to define your own? Let's first go over when a destructor is called and used.
You probably already know that variables have scopes.
If you define a variable inside a function or a loop (these are called local variables)
and try to access outside of the function of loop (or its scope), the compiler is unhappy.

.. code-block:: c++

   while (i < 5)
   {
      int foo;
   }
   foo = 6;   // error! (assuming you don't have another variable named foo that was defined outside of the loop)

---------------

.. code-block:: c++

   int foo(){
   bool bar;}
   main()
   {

      bar = true;  // error!
   }

Why does this happen?
What going out of scope really means is that you are trying to access a variable after it was already destructed!

.. code-block:: c++

   while (i < 5)
   {
     int foo;
   }          // foo's destructor is called here, after the end of this block
   foo = 6;

---------------

.. code-block:: c++

   int foo()
   {
      bool bar;
   }     // bar's destructor is called here

   main()
   {
      bar = true;  // error!
   }

Okay.
We get it.
So why would you need to define your own destructor?
Because dynamically allocated variables (or anything created with the new command) will not be destructed automatically by the compiler.
Well why not, you scream at your computer. When you create dynamically allocated variables or arrays, they are created in memory in the heap, whereas all other variables are created in the stack.
The heap and the stack work differently.
The heap is basically extra space that you can rent and use at your own discretion; that is, you are responsible for what you put in there, not the compiler.
The compiler takes care of things in the stack.
If you don't delete variables that are in the heap after you are done with them, they will stay there, using up memory with out you realizing
(this is called a memory leak).
Side note: According to the lecture notes you should also have a destructor whenever you open a disk file or connect to another computer over the network, but we haven't done anything with that.

Let's look at the C++ code for a destructor.
The name of a destructor must be the name of the class (like the constructor) but with a tilde (~) in front of it, and no return type.
Now you finally get use for that key on your keyboard! But unlike the constructor, the destructor has NO parameters and you can have only one in every class.

Here's an inline destructor for the class Foo:

.. code-block:: c++

   ~Foo()
   {}

And here it is outside of the class:

.. code-block:: c++

   Foo::~Foo()
   {}

You delete a dynamically allocated variable or array with "delete" or "delete []".
If use ``new`` command you have to use ``delete`` to avoid memory leak.
If use have ``[]`` after the ``new`` command you have to use ``delete []`` to avoid memory leak.

For example,

.. code-block:: c++

   Foo ptr* = new Foo;

later needs

.. code-block:: c++

   delete ptr*;

and,

.. code-block:: c++

   Foo ptr* = new Foo[12];

later needs

.. code-block:: c++

   delete[] ptr*;


If you use the word ``delete`` outside of the destructor, you are also calling the destructor.
For what variables is the destructor called?
Note that it will always be called after the last brace of the variable's scope.

1) Destructor is called for every element of an array that is of type class.

.. code-block:: c++

   if (1)
   {
      Foo arr[33];
   }  // Destructor is called 33 times at the end of the function (after the last brace)

2) Destructor is called for an instance or object of type class.

.. code-block:: c++

   if (1)
   {
      Foo bar;
   } // Destructor is called

Destructor is NOT called for pointers of class type.

.. code-block:: c++

   if (1)
   {
      Foo *ptr;
   } // Destructor is not called


But be careful; you can have memory leaks if you do not use ``delete`` properly.

Memory leak below:

.. code-block:: c++

   if (1)
   {
    Foo *ptr = new Foo("foo");
   } // pointer is destroyed because it goes out of scope but not the object it pointed to. memory leak!!!

No memory leak below:

.. code-block:: c++

   if(1) {
    Foo *ptr = new Foo("foo");
    ...
    delete ptr; // object pointer points is deleted; no memory leak!
   } // pointer is destroyed because it goes out of scope

No memory leak below:

.. code-block:: c++

   if(1) {
    Foo foo("foo");
   } // no memory leak, object goes out of scope
