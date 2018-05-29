.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

=========================
Abstract Data Types (ADT)
=========================

The first topic is on Abstract Data Types (ADT). 
But before we go into what that is, we need to look at the three components that make up an ADT:
(1) data structure,
(2) algorithm, and 
(3) interface.

1) Data Structure
-----------------

A data structure is basically just a bunch of data that you want to do something with.
That data can be 100 random numbers, the first five prime numbers, the names of all the characters in The Big Bang Theory, or a items in a grocery list. 
How do you store this data in C++?
You can use an int, an array of strings, etc.

.. code-block:: c++

   int numberOfFriends = 5;
   string BigBang[5] = { "Sheldon", "Leonard", "Raj", "Howard", "Penny"};

But what do you do with these data structures?
That's where the algorithm comes in.

2) Algorithm
------------

An algorithm is a fancy word for a set of instructions that when implemented solve some problem.
What kind of problem? Any problem that is related to data manipulation.
The data the algorithm will be using is called the input, and the result the algorithm produces is the output.
Here is the implementation of a very simple algorithm that tells you whether you can drink alcohol in the U.S.

.. code-block:: c++

   cin >> age;
   if (age >= 21)
       cout << "Yes";
   else
       cout << "No";

Notice how I specified that this is the implementation of an algorithm.
When we use programs that solve some problem, we don't see the implementation of the program.
Why not? Because we don't need too.
The program will still work without us knowing, so it's unnecessary (and even risky, as we will see later).
The same goes for the data structure.
In the example above, hiding the information may not seem so important as the algorithm is very simple,
but with programs that have algorithms+data structures that are much more complex, like Facebook,
it would be useful and much more usable to hide it.
So how do we mask the gory details?

3) Interface
------------

The interface is the walls around the algorithm and data structure.
The user of the program can interact with the program through specially designed functions that are user-friendly
(that is, anyone who doesn't know the gory details should still be able to use it. 
Imagine trying to use Microsoft Word without the interface, very few people would actually be able to use it).
The interface also prevents people from stealing or messing with the code.
Together, these make up an ADT.
Programs are made up of multiple ADTs, each of which solve a small problem of the puzzle.
By the way, C++ is an object oriented programming language, meaning it is modeled on ADTs!
You will see this illustrated below.

So what do we do with these ADTs in C++?
ADTs translate to C++ classes. The data structures and algorithms, which need to be private, 
translate to the private data members and functions of the class
(by the way, for some reason, we have only been making data members private, not the functions.
The examples will be based on what we learned).
The interface translates to the public functions.
In C++, the private functions can only be accessed by objects of the class, while the public functions can be accessed by anyone.
Perfect!
Here is an example illustrating how we define each of the components of an ADT in a class.

.. code-block:: c++

   class Alcohol
   {
   public:
        Alcohol(int age)     // The initializing interface
        {
             m_age = age;
        }
        bool legal()          // The interface
        {
             if (m_age >= 21)     // The algorithms are hidden
                 return true;
             else
                 return false;
        }
   private:
        int m_age;     // The data structure
   }
   
   int main()
   {
        Alcohol Bob(16);     // Using the interface
        legal();             // returns false
   }