.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

========
Pointers
========

Every variable and array that you create has an address in memory.
If the variable takes up more than one memory space, its address is the first space it occupies.
Where exactly the variable is in memory is basically random (except in arrays, where each of the elements are next to each other in memory).
If you want to get the address of a variable you just stick an ampersand & in front of it. But why would you even care about where the variable is in memory?
You can use variables' addresses to access variables indirectly with pointers.
What do I mean by indirectly? Let's first look at what pointers are.
A pointer is a variable whose sole purpose is to hold another variable's address.
The pointer itself has its own address in memory, but we don't care about that.
Just remember that the pointer and the variable whose address it stores are not aliases for the same variable!
Though you can access and/or change the value of the variable using either, you can also change the memory address that the pointer holds (or what the pointer is pointing to).
What's the use of pointers?
One of the things pointers allow you to do is change the value of a variable across functions.
If you want to change a variable's value in a function different from the one you declared it in, you can have it take a pointer as a parameter and pass into it the address of the variable.
That way you are changing the variable itself, not a local variable.
You can also use pointers to access the variables of an array (the name of an array by itself is actually a pointer).
You can also access structures or classes with a pointer of type class and dynamically allocated variables.
Now let's look at the actual code.
To declare a pointer, you write the type of the variable the pointer will be pointing to, a star ``*``, and whatever you want to pointer's name to be.

.. code-block:: c++

   int *foo;

But wait! You aren't done! Now, before you try to access the value of the variable that the pointer points to,
you need to assign it an address to point to! Otherwise, if left uninitialized, the pointer will point to some random place in memory.
This can be very very bad if you try and change its value!

.. code-block:: c++

   int bar = 5;
   int *foo = bar;      // WRONG!!!!

Remember, you need to use an ampersand & before the variable to access its address.

.. code-block:: c++

   int bar = 5;
   int *foo = &bar;     // Correct!

Now, say you want to assign a pointer a variable on a line separate of its definition. Do you do this?

.. code-block:: c++

   int *foo;
   int bar = 5;
   *foo = &bar;    // WRONG!!!

You only use the star * in two situations with a pointer:
1) when you are declaring a pointer variable and
2) when you want to access the value of the variable it points to.
Confusing, right? Since we want to set the pointer foo to bar (rather, the address of bar), we don't use the star ``*``.

.. code-block:: c++

   int *foo;
   int bar = 5;
   foo = &bar;    // Right!

Now let's access the value of bar using foo and change it to 6.

.. code-block:: c++

   int *foo;
   int bar = 5;
   foo = &bar;
   *foo = 6;       // aka bar = 6
   cout << *foo << endl;   // prints out 6
   cout << bar << endl;    // prints out 6

Pointers can also point to arrays and behave like the array! Let's look.

.. code-block:: c++

   string BigBang[4] = { "Sheldon", "Leonard", "Howard", "Raj"};
   string *foo = BigBang;

Wait, you scream! You're missing the ampersand! Well, you actually don't need the ampersand when pointing to arrays.
But this is NOT because the array is actually a pointer.
C++ allows this so you can be able to use an array like a pointer (I don't know if there is actually a better explanation, but that's what we were told).
Just remember that assigning a pointer to an array is the exception to the rule of the ampersand.
Now let's see how we can access the values of the elements of the array with the pointer.

.. code-block:: c++

   string BigBang[4] = { "Sheldon", "Leonard", "Howard", "Raj"};
   string *foo = BigBang;
   cout << foo[0] << " and " << *(foo + 1) << " are roommates.";
   // prints out "Sheldon and Leonard are roommates."

Notice that you can use either the pointer name and brackets with the element name OR
the pointer name plus the number of elements to go forward surrounded by parentheses and the star.
If you didn't include the star in the second option, you would actually be moving the pointer forward from one element to the next.

.. code-block:: c++

   cout << *(foo + 1) << endl;   // Prints out "Leonard"
   cout << foo + 1 << endl;      // Prints out the address of foo[1] (or would it be foo[2] because you move one more from before?)

You can also use pointers to access and/or change the contents of an instance of a structure or class.
Here is a sample struct and instance without using a pointer to the struct:

.. code-block:: c++

   struct Physicist
   {
        string field;
        int comicBooks;
   };
   int main()
   {
        Physicist raj;
        raj.field = "Astrophysics";
        raj.comicBooks = 79;
   }

Here is the same struct and instance but with a pointer to the instance.
Notice that you can't use just the dot operator when trying to modify a variable in the struct or class the pointer is pointing to.
You must either use the value of the pointer in parentheses (because of the precedence rules) and then the dot operator--(``*foo``).--
or just the name of the pointer and the arrow operator--``foo->``. 
These do the same thing.
 
.. code-block:: c++
 
   struct Physicist
   {
        string field;
        int comicBooks;
   };
   int main()
   {
        Physicist raj;
        Physicist *foo = &raj;
        (*foo).field = "Astrophysics";
        foo->comicBooks = 79;   // this method is preferred
   }

So why would we use a pointer to the object instead of just the object?
In this case, it doesn't matter, but if you add a function that accesses and/or changes the contents of the struct, you need to use the pointer version!
Why?
Because if you don't pass the address of the object to the function, you would be changing the contents of a local variable (i.e. once the function finishes all the work will be gone). And the only (or rather best) way to pass in the address of the object to a function is for the parameter to be able to hold an address.
And what variable does that?
A pointer!
Let's see how it works by adding a function to our class.

.. code-block:: c++

   struct Physicist
   {
        string field;
        int comicBooks;
   };
   void shoppingTrip(Physicist *bar)  // bar takes in an address of a variable of type Physicist
   {
        bar->comicBooks += 10;
   }
   int main()
   {
        Physicist raj;
        Physicist *foo = &raj;
        (*foo).field = "Astrophysics";
         foo->comicBooks = 79;
         shoppingTrip(&raj);   // Note that you need to include the & to pass the address
   }

You can also do this with classes.
Because classes can have variables and functions, you can access a function that belongs to an object with a pointer.

.. code-block:: c++

   class Physicist
   {
   public:
       void shoppingTrip()  // Don't need to have a parameter of type pointer to Physicist because the function is now in the object's definition 
       {
             comicBooks += 10;
       }
   private:
        string field;
        int comicBooks;
   };
   int main()
   {
        Physicist raj;
        Physicist *foo = &raj;
        (*foo).field = "Astrophysics";
         foo->comicBooks = 79;
         foo->shoppingTrip();
   }

The final thing you can do with a pointer is to hold the address of a dynamic variable or array.
Be sure to delete the pointer that points to the dynamically allocated variable or array once you are done!
Why?
Because you need to free the memory because it is in the heap
(recall from before that the compiler doesn't free the memory on the heap for you).

.. code-block:: c++

   string *foo;
   cin >> bar;
   foo = new string[bar];
   foo[0] = "Sheldon";
   foo[1] = "Leonard";
   delete [] foo;
   --------------
   string *foo;
   foo = new string("Batman");
   delete foo;
   -----------
   Physicist *foo;
   foo = new Physicist("Sheldon");
   delete foo;

Void Pointers
-------------

.. code-block:: c++

   void *p;

is a pointer which can be used to assign any variable.

.. code-block:: c++

   int i;
   float f;
   string s;
   class c;
   
   p = &i;
   p = &f;
   p = &s;
   p = &c;

are ok.

Fuction Pointers
----------------

You can also have pointer to a function:

.. code-block:: c++

   int pee(){ cout << "pee" << endl }
   int poop(){ cout << "poop" << endl }
   
   int (*p)();
   
   if(1) p = &pee();
   else p = &poop();
   
   p(); // will execute the function
   (*p)(); // will execute the function

or if there are arguments

.. code-block:: c++

   int pee(int p){ cout << "pee" << i << endl }
   int poop(int p){ cout << "poop" << i << endl }
   
   int (*p)(int);
   
   if(1) p = &pee;
   else p = &poop;

   p(1); // will execute the function
   (*p)(1); // will execute the function
   
``int (*p)(int)`` can be a pointer to any function that returns integer and has one integer argument.
