.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.


Templates
=========

Now let's look at templates.
You use templates to create a stand-in type that would allow you to use any type of variable in a function or class.
This will create a generic function.
The syntax for a template function is the word ``template``, the word ``typename`` followed by whatever name you want, all of which is within angular brackets.
You replace any variable type names in the function with your template name.
Here is a function that takes in any type of variable (called Foo) and prints it out.
Note that one restriction of template functions is that you must have *one* of the parameters be of type template typename.

.. code-block:: c++

   template <typename Foo>
   void print(Foo a)
   {
        cout << a << " exists!" << endl;
   }

Now you can use this one function to print out an int, string, float, bool, etc. (this function has some restrictions concerning more complex variables, but for most simple variables it works).

.. code-block:: c++

   int x = 5;
   print(x);
   string d = "Sam";
   print(d);
   float g = 3.44;
   print(g);

Whenever you use the template function for a different type of variable (not necessarily for every new variable),
the compiler creates a copy of the function, replacing every template typename with that variable's type.
For this reason, if a template function has more than one parameter of type template typename, the variables you pass in *MUST* be of the same type.
You can also override a template function with a regular one (meaning that it would have the same name and parameters).
Multi-type templates are also a thing;
this is when you have a function with two *DIFFERENT* template type names.
You can then pass in two variables of different types (order doesn't matter) or the same type.

Generic classes are like generic functions and can hold any and do stuff with any type of variable.
Just add the same template header as for a function.
One thing to note, however, is that if you want to put the class definition outside of the class,
you need to include the template header and the name of the template typename with the class name.
Here is an example:

.. code-block:: c++

   template <typename Foo>
   class Bar
   {
   public:
      void set(Foo a);
      void print()
      {
          cout << m_a << " exists!" << endl;
      }
   private:
      Foo m_a;
   };

   template <typename Foo>
   void Bar<Foo>::set(Foo a)
   {
       m_a = a;
   }

If you want your function to be treated as if it is inline, add "inline" after the template header.

.. code-block:: c++

   template <typename Foo>
   inline
   void Bar<Foo>::set(Foo a)
   {
       m_a = a;
   }
