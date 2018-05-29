.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

=================
Class composition
=================

Class composition is when a class has one or more member variables that are objects, aka variables that are of type class.
This includes strings (string is a class).

.. code-block:: c++

   class Scientist
   {
   private:
      Book b;
      TV s;
      int numComicBooks;
   }

Why do we care about this? When you have member variables that are objects, they have their own constructors. But when and how are their constructors called? Luckily, when you have non-scalar variables, C++ automatically calls their (default) constructors. This happens before the main class' constructor runs, because you may be using the member variables in it, and those objects need to be constructed.

.. code-block:: c++

   class Scientist
   {
   public:
      Scientist()
   // b's constructor called here by the compiler
   // s's constructor called here by the compiler
      {
            b.read();
            s.watch();
            numComicBooks = 67;
      }
   private:
      Book b;
      TV s;
      string name;
      int numComicBooks;
   }
   class Book
   {
   public:
       Book()
       {
             pages = 100;
      }
   }
   class TV
   {
   public
       TV()
       {
              show = "Star Trek";
       }
   }

Note that the objects' constructors are called in the order in which they appear in the class definition.
What about the destructor? Since the destructor is allowed to use member variables, you can't destruct any member variables objects until after the destructor finishes, meaning they get destructed after the class itself.

.. code-block:: c++

   Scientist::~Scientist()
   {
      ......
   }   // class destructed after this brace
   // s destructed here
   // b destructed here
   
   Note that the objects' destructors are called in reverse order from the order in which they appear in the class definition.
   But remember, this automatic constructor/destructor calling ONLY happens with non-scalar member variables. If you have scalar member variables, like ints, bools, doubles, etc., you have to initialize them. So if you want to use, for example, a number of comic books variable without initializing it in the constructor, you'll get random values!
   
   If you have  an even more complicated class composition, i.e. a class' object member variable also has a member variable that is an object, the object "deepest in" will be constructed first and destructed last. Here is a simple example.
   
   int main()
   {
       Person sheldon;   // #1, sheldon will be constructed...
   }
   class Person
   {
      Person()   // #2, but sc must first be constructed
       {}              // #6, sheldon can now be constructed!
      ~Person()
       {}
     Scientist sc;
   }
   class Scientist
   {
      Scientist()    // #3, but ph must first be constructed
       {}                // #5, sc can now be constructed!
      ~Scientist()
       {}
      Physicist ph;
   }
   class Physicist
   {
      Physicist()  // #4, ph is constructed!
      {}
      ~Physicist()
      {}
   }

Here is is again but for destructors:

.. code-block:: c++

   int main(){
       Person sheldon;
   }  // #1, function finished, now sheldon needs to be destructed...
   
   class Person
   {
      Person()
       {} 
      ~Person()
       {}              // #2, sheldon is destructed. #3, but now sc must be destructed
     Scientist sc;
   }
   class Scientist
   {
      Scientist()
       {}
      ~Scientist()
       {}              // #4, sc is destructed. #5, but now ph must be destructed
      Physicist ph;
   }
   class Physicist
   {
      Physicist()
      {}
      ~Physicist()
      {}              // #6, ph is destructed.
   }

Okay, so now it appears everything works.
But what if one of your objects only has one constructor and it requires a parameter?
How do you pass them in? C++'s automatic calling won't work now because you have to tell it what values you want to be passed.
You do this by creating an initializer list.
An initializer list consists of the name(s) of the object(s) and its/their parameter(s), put after the constructor name.
After the constuctor's name is a colon and the objects are separated by commas.
Don't put a semicolon or anything else after the list!

.. code-block:: c++

   class Scientist
   {
   public:
      Scientist() : b("Theory of Everything"), s(4)
      {
            b.read();
            s.watch();
            numComicBooks = 67;
      }
   private:
      Book b;
      TV s;
      string name;
      int numComicBooks;
   }
   
   class Book
   {
   public:
       Book(string name)
       {
             title = name;
             pages = 100;
      }
   }
   
   class TV
   {
   public
       TV(int favSeason)
       {
              show = "Star Trek";
              season = favSeason;
       }
   }

The initializer list should only be included in the place where you define the function, whether that be in the class or outside of it.

INSIDE THE CLASS:

.. code-block:: c++

   class Scientist
   {
   public:
      Scientist() : b("Theory of Everything"), s(4)
      {
            b.read();
            s.watch();
      }
   private:
      Book b;
      TV s;
      string name;
      int numComicBooks;
   }

OUTSIDE THE CLASS:

.. code-block:: c++

   class Scientist
   {
   public:
      Scientist();   // NO initializer list here
   private:
      Book b;
      TV s;
      string name;
      int numComicBooks;
   }
   
   Scientist::Scientist() : b("Theory of Everything"), s(4)
      {
            b.read();
            s.watch();
      }

You can also initialize scalar member variables in the initializer list if you wish.
The parameters don't even have to be constant.

.. code-block:: c++

   Scientist::Scientist() : b("Theory of Everything"), s(n), numComicBooks(67)
      {
            b.read();
            s.watch();
      }

And you don't have to worry about your non-scalar member variables being initialized;
C++ will still automatically do initialize them, regardless of whether or not you have an initializer list.