.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

============
Polymorphism
============

Polymorphism is when you use a pointer or reference to a base class to access a derived class.

.. code-block:: c++

   class Foo {};
   class Bar : public Foo {};
   int main()
   {
       Bar b;
       Foo *f = &b;    // Okay!
   }

But you CANNOT use a pointer of type derived class to access the base class!

.. code-block:: c++

   int main()
   {
       Foo f;
       Bar *b = &f;    // NO!
   }

Now let's see how we would use polymorphism. Say we have two classes: Person and Wizard. Wizard is derived from Person. 

.. code-block:: c++

   class Person
   {
   public:
        string getName();
        string getJob();
   private:
        string m_name;
        string m_job;
   };
   
   class Wizard : public Person
   {
   public:
        string getWand();
   private:
        string m_wand;
   };

And we also have a function (outside of the class) that takes in a pointer to a Person. 

.. code-block:: c++

   void party(Person *foo)
   {
       cout << foo->getName << " is not working as a " << foo->getJob << " right now!" endl;
   }
   
We can make both a Person and a Wizard party (party()). Why?
Remember that all instances of Wizard inherit a name and a job from the Person class.
When you pass the address of a Wizard to a Person pointer, the compiler just
checks whether the Wizard has the same public functions as a Person;
if it does, it must be a Person.
Hence the compiler doesn't even know that the class is more than a Person.
   
.. code-block:: c++
   
   int main()
   {
       Person foo("Bob", "Builder");
       Wizard bar("Neville", "Herbology professor");
       party(&foo);
       party(&bar);
   }

You can use polymorphism to make more efficient functions instead of writing duplicate code, like we did above.
You only need to create one function instead of two separate ones, one for a Person and one for a Wizard.
You can also do the same thing by using the base class to access two or more derived classes.
Let's say we want to have a function that takes in a student of any major and outputs what they will do after they graduate. 

.. code-block:: c++

   void future(Major &m)
   {
       cout << m.getName() << " will " << m.execute() << endl;
   }

That means we want our Major class to have two virtual functions, getName and execute, that all derived classes will inherit.

Here's the first draft of the class Major.

.. code-block:: c++

   class Major
   {
   public:
       virtual string getName();
       virtual string execute();
    };

What do we do with the body of Major's getName?
Since we are not planning on creating an instance of Major (we don't want anyone to have a generic major), it shouldn't return anything.
But we still need to have it, otherwise our future function wouldn't work
(the compiler wouldn't be able to access m.getName() or m.execute() because neither of them exist in m).
Let's just make it return an empty string for now.

.. code-block:: c++

   class Major
   {
   public:
       virtual string getName()
       {
              return ("");
       }
       virtual string execute()
       {
              return ("");
       }
    };

Now let's make some derived classes. Our derived classes are going to redefine the inherited functions (specialization).

.. code-block:: c++

   class Philosophy : public Major
   {
   public:
       Philosophy (string nm)
       {
           m_name = nm;
       }
       virtual string getName()
       {
           return m_name;
       }
       virtual string execute()
       {
              return ("think");
       }
   private:
       string m_name;
   };
    
   class Art : public Major
   {
   public:
       Art (string nm)
       {
           m_name = nm;
       }
       virtual string getName()
       {
           return m_name;
       }
       virtual string execute()
       {
              return ("try not to starve");
       }
   private:
       string m_name;
   };

Now when we create instances of Philosophy and Art, we can call the future function and everything will work.

.. code-block:: c++

   int main()
   {
       Philosophy a("Betty");
       Art b("John");
       future(a);  // Outputs: "Betty will think"
       future(b);  // Outputs: "John will try not to starve"
   } 

The beauty of polymorphism is that you can add as many derived classes of Major as you like and you wouldn't have to rewrite the future function!

Let's look at the Major class again.
Notice how it isn't doing anything useful? C++ actually has special terminology and syntax for so called "dummy" base classes.
They are called pure virtual functions; their functions are never called and they don't do anything.
Instead of writing extra lines of code that don't do anything, instead you can just write " = 0;" after the function name.
This is the same for all pure virtual functions, no matter what their return type is. 

.. code-block:: c++

   class Major
   {
   public:
       virtual string getName() = 0;
       virtual string execute() = 0;
    };

When you define pure virtual functions in the base class, you are forcing all of your derived classes to define their own version of it, which could also be a pure virtual function if you want.
Here's some more terminology.
If you have at least one pure virtual function in a base class, it is called an Abstract Base Class (ABC).
What's the point of having ABCs? It forces whoever is making the derived classes to define a version of every pure virtual function.
This prevents bugs that can occur when you try and access a derived class' function.

One more topic: say you want your base class to use dynamic variables and you need to define your own destructor, in both the base and derived class.
When defining a destructor in classes that use inheritance and/or polymorphism you MUST make them virtual! 
Why?
Let's see an example without using virtual destructors.

.. code-block:: c++

   class Foo
   {
       ....
       ~Foo() { delete x; }
   };
   class Bar : public Foo
   {
       ....
       ~Bar() { delete y; }
   };
   int main()
   {
       Foo *f = new Bar b;   // haha, "new barbie"
       ...
   }
   /*
   When the compiler gets here, b needs to get destructed, but f can only access it through class Foo. 
   But since ~Foo() isn't declared as virtual, f doesn't know that b has its own destructor, so it just calls ~Foo() and that's it.
   But now there's a memory leak because we didn't deallocate the memory in b!
   */

Let's see what happens when you make the destructors virtual.

.. code-block:: c++

   class Foo
   {
       ....
       virtual ~Foo() { delete x; }
   };
   class Bar : public Foo
   {
       ....
       virtual ~Bar() { delete y; }
   };
   int main()
   {
       Foo *f = new Bar b;
       ...
   }
   /*
   When the compiler gets here, f calls ~Foo(), but since it is labeled virtual,
   it also continues to delete the most derived version of the object it points to, so it does call ~Bar().
   No memory leak!
   */

That's it!