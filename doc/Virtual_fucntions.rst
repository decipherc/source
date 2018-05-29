.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

=================
Virtual functions
=================

You use virtual for a function on these three conditions:

(1) you are creating derived classes based on the class of the function;

(2) you want to change the implementation of that function in the derived class; and

(3) you want the compiler to use the most derived version of the class when using polymorphism
(when you reference a derived class with a pointer or reference to a base class).

You CANNOT make a constructor virtual, but you SHOULD make destructors virtual!

Consider this class:

.. code-block:: c++

   class Character
   {
   public:
       Character(string name)
       {
           m_name = name;
           m_action = "rushes toward the enemy";
       }
       string name() const
       {
           return m_name;
       }
       virtual void printWeapon() const = 0;
       virtual string attackAction() const
       {
           return m_action;
       }
       virtual ~Character() {}
   private:
       string m_name;
       string m_action;
   }; 
 
Character()
-----------

``Character()'' is not virtual because it is a constructor.

name()
------

Let assume that there is the following requirements:
No two functions with non-empty bodies may have implementations that have the same effect for a caller.
For example, there's a better way to deal with the ``name()'' function than to have each kind of character declare and identically implement a name function.
``name()'' is not virtual for two reasons
(and these are just because of the requirements of the spec, not that you shouldn't do it in general).

One of the ways to return the name for that function is to do this for every derived class:

.. code-block:: c++

   virtual string name()
   {
        return m_name;
   }

and make Character::name() (pure) virtual as well (technically you only need to write "virtual" before the base class version, but it's better practice to use virtual for the derived class as well):

.. code-block:: c++

   virtual string name() = 0;

This would work, but it is a repetition of implementation to have the same ``name()'' in all of the derived classes, so we can't do it.
What do we do?
We need to make the derived classes inherit the correct name function, so we won't even need to redefine the name function in each of them.
Now, if you try and just do this in the Character class,

.. code-block:: c++

   string name()
   {
        return m_name;
   }

This is not going to work because the ``Character'' class can't access a derived classes' private member variables.
So now what?

Thanks to another seemingly random spec requirement:

The ``Character'' class must not have a default constructor.
The only constructor you may declare for Character must have exactly one parameter.
That parameter must be of a builtin type or of type string, and it must be a useful parameter.

We can get the name of the Dwarf/Elf/Boggie from the constructor!
Simply assign Character's m_name variable to it!
But wait?
How does Character know what their name is?
Aren't you creating a new variable with the Dwarf/Elf/Boggie constructor?
You are, but FIRST, the constructor of the base class (or possibly other derived classes that come before it) is constructed first!
That's why we can put an initializer list in before all of the derived class' constructors,
because Character's constructor is called first and it doesn't (can't) have a default constructor.
Yay! So we made ``name()'' work.

printWeapon()
-------------

``printWeapon()'' is a pure virtual function.
This is because, unlike ``name()'',
we don't have to worry about repeating the implementation because we are printing out three distinct strings of weapons for each derived class.
Note that this is allowed by the spec:
Notice that { return "rushes toward the enemy"; } and { string s("rushes toward"); return s + " the enemy"; } have the same effect. And notice that { cout << "a pointed stick"; } and { cout << "banana"; } do not have the same effect.

attackAction()
--------------

``attackAction()'' is a virtual function.
Why?
First of all, we need to be sure we are not repeating an implementation, as both a Dwarf and an Elf "rushes toward the enemy", so we can't just do 

.. code-block:: c++

   virtual string attackAction()
   {
          return "rushes toward the enemy";
   }

for both Dwarf and Elf. So what can we do? Notice that only a Boggie "whimpers" as an attack, so it is okay to do

.. code-block:: c++

   virtual string attackAction()
   {
          return "whimpers";
   }

in the Boggie class.
So why don't we just make Character's attackAction be, by default, "rushes toward the enemy", so we can just redefine it for the Boggie class?
Note though, that Character's attackAction MUST be virtual so we can redefine it, otherwise a Boggie will also return "rushes toward the enemy".
So how do we implement it?
We can just define a private member variable in Character called m_action, define it in the constructor to be equal to "rushes toward the enemy", and for the attackAction in Character do this:

.. code-block:: c++

   virtual string attackAction()
   {
          return "rushes toward the enemy";
   }

and still do this in the Boggie class:

.. code-block:: c++

   virtual string attackAction()
   {
          return "whimpers";
   }


When the compiler calls a virtual function, it will call the most derived version.
So when a Boggie calls attackAction, it will use Boggie's redefined attackAction,
and when a Dwarf or an Elf calls the action, it will do Character's version because they don't redefine it.

~Character()
------------

~Character is virtual because a destructor should always be virtual when using inheritance.
It doesn't do anything in the Character class because it doesn't need to (no dynamic variables, no requirements by spec).
Each of the derived class' destructors output the required lines.

Note: The reason I created a m_weapon variable in Dwarf to hold "an axe" instead of just outputting it like in the other functions
is solely for the purpose of keeping all of the functions (in this case constructors) to have different implementations.
I'm not sure if it would count as repetition, but just in case.

Note that because Character has at least one pure virtual function (printWeapon),
that it makes it an ADT, and thus you cannot construct an object of type Character, which goes under this requirement:

Although the expression new Elf("Orlon", 8) is fine, the expressions new Character("Goodgulf") and new Character(0) must produce compilation errors.
(A client can create a particular kind of character object, like an Elf object, but is not allowed to create an object that is just a plain Character.)