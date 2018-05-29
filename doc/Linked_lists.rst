.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

============
Linked Lists
============

Recall from the first email about ADTs that every ADT has a data structure, algorithm, and interface.
In this email I will be talking about a linked list, which is a type of data structure.

A linked list is like an array except every element consists of dynamically-allocated variable and pointer; this is called a node.
Unlike an array, the nodes aren't next to each other in memory, instead they are scattered randomly through out memory.
How are the connected? Every node's pointer points to the next one.
The list starts with a head pointer that points to first node, and the list ends with the last node pointing to null pointer.
Here is what the code looks like.

First things first, let's create a struct for the items we want to store.
This struct will be the type for the node.
Remember that every node needs some amount of variables and a pointer of type struct.
In this example we will only have two variables, but you can have as many and with whatever types you want. 

.. code-block:: c++

   struct Scientist
   {
        string name;
        string field;
        Scientist* foo;     // foo will point to the next Scientist item
   };

Why is the pointer of the same type as the struct itself?
The pointer needs to be able to hold the address of another struct of the same kind. It is NOT pointing to itself.

Let's try creating some instances of our struct. First we need to declare a head pointer.
When creating the instances, it is typical to dynamically allocate them. Because of that, we need additional pointers to hold our Scientists instances.

.. code-block:: c++

   int main()
   {
        // Pointers holding the nodes
        Scientist* head = new Scientist;
        Scientist* a = new Scientist;
        Scientist* b = new Scientist;
   
        // Assigning the variable and pointer of the first node, pointed to by the head pointer
        head->name = "Sheldon Cooper";
        head->field = "Theoretical physics";
        head->foo = a;  // Pointer points to the next node, a
   
        // a node
        a->name = "Howard Wolowitz";
        a->field = "Engineering";
        a->foo = b; // Pointer points to the next node, b
   
        // b node
        b->name = "Amy Farrah Fowler";
        b->field = "Neurobiology";
        b->foo = nullptr; // Pointer points to nullptr, no more nodes
   
       // Be sure to delete the pointers because they are using memory on the heap
        delete head;
        delete a;
        delete b;
   }

This feels tedious to use main to create our instances.
Why don't we just create a linked list class?
A linked list class is like a class but with only one member variable: the head pointer.
We can add public functions that can perform basic operations on the linked list.
I'll go over them in more depth below.

.. code-block:: c++

   struct Scientist
   {
        string name;
        string field;
        Scientist* foo;     // foo will point to the next Scientist item
   };
   class BigBang
   {
   public:
       BigBang();       // constructor
       void addToFront(string n, string f);
       void addToRear(string n, string f);
       void deleteScientist(string n);
       bool findScientist(string n);
       void printScientists();
       ~BigBang();       // destructor
   private:
       Scientist* head;
   }
   
   int main()
   {
        addScientistToRear("Sheldon Cooper", "Theoretical physics");
        addScientistToRear("Howard Wolowitz", "Engineering");
        addScientistToRear("Amy Farrah Fowler", "Neurobiology");
   
        deleteScientist("Howard Wolowitz");
        findScientist("Raj Koothrappali");    // returns false
        printScientists();
   }

Because this is a class, we need a constructor.
The only member variable in the class is head.
We want to initialize it to nullptr because 
a) you shouldn't use a pointer without initializing it because bad things can happen and 
b) this also means that we are initializing it to an empty list.

.. code-block:: c++

   BigBang::BigBang()
   {
       head = nullptr;   // if head is equal to nullptr, the list is empty
   } 
   
We can also write a public function that can add a node depending on where we want it.
Depending on where it is, you have to adjust different pointers and you may or may not have to traverse part or all of the array.
If you were to add an item to an empty list, you could call either function, as addToRear transfers you to addToFront if the list is empty.

.. code-block:: c++

   void BigBang::addToFront(string n, string f) // parameters for name and field
   {
       Scientist *node = new Scientist;  // Create a new Scientist, node points to it
       node->name = n;     // Change the item's variables through the pointer
       node->field = f;
   
       node->foo = head;    // Sets node equal to the node that the head points to (remember that head still points to the previous top node). 
   
       head = node; // Sets head to point to the same node as the temporary one (aka the new top node).
   }
   
   void BigBang::addToRear(string n, string f)
   {
       if (head == nullptr)   // if the head points to nullptr, aka empty list
           addToFront(n, f);
       else
       {
           Scientist *ptr = head;  // create a temporary pointer
           while (ptr->foo != nullptr) // while the pointer of the node that the pointer points to is not nullptr, aka while it's not at the last node
               ptr = ptr->foo;  // keep on traversing the list through the nodes' pointers
           Scientist *node = new Scientist;  // create new item
           node->name = n;   // set variables
           node->field = f;
   
           node->foo = nullptr;   // since this is the last item, its pointer points to nullptr
       }
   }

Notice that to avoid going past the last node in the while loop, you need to use (ptr->foo != nullptr).
This will stop moving the pointer once it reaches the last node.
Quick note: you can NOT use ptr++ to traverse a linked list.
Why? 
``ptr++`` moves the pointer forward in memory how ever many bytes are in Scientist variable,
and there is NO guarantee that the next node of a linked list is at that memory address.
Remember, this is not an array, so the nodes are not next to one another in memory.

Let's add a function that traverses the list and prints out every Scientist.

.. code-block:: c++

   void BigBang::printScientists()
   {
       Scientist *ptr = head;    // temporary pointer, starts out pointing to the same node as head
       while (ptr != nullptr)     // while the pointer isn't at the end
       {
           cout << "Name: " << ptr->name << "Field: " << ptr->field << endl;
           ptr = ptr->foo;      // continue traversing
       }
   }

Notice here that we're using (ptr != nullptr), not (ptr->foo != nullptr).
Why is that?
Here it's okay to go past the last node because we aren't looking for it specifically, we just want to print out the values.
The compiler will run through the loop, and once it goes to nullptr, it will say "well, that's the end of that of the list.
There is nothing else to do. 
But in the case of the previous function,
we couldn't have left the pointer to go to nullptr because by then the pointer would've have gone too far and
we wouldn't be able to backtrack to the previous node.
This is one of the characteristics of linked lists (or at least the form we are looking at now):
going through the list is a one way street and if you want to reach an item at the bottom, you need to go traverse the whole list. 

Here's a function that finds a Scientist (I wrote this one; it's correct, right?).

.. code-block:: c++

   bool BigBang::findScientist(string n)
   {
       Scientist *ptr = head;   // temporary pointer
       while (ptr != nullptr)
       {
           if (ptr->name == n)
                  return true;
           ptr = ptr->foo;
       }
       return false;
   }

Here's a function that deletes a Scientist.

.. code-block:: c++

   void BigBang::deleteScientist(string n)
   {
       Scientist *ptr = head;    // temporary pointer
       while (ptr != nullptr)   // until it's at the end of the list
       {
   // Stop the loop if the next node isn't a nullptr and the next node's name is a match; stop at the node above the selected node 
           if (ptr->foo != nullptr && ptr->foo->name == n)
               break;     
           ptr = ptr->foo;
       }
       if (ptr != nullptr)     // Check to make sure it isn't at the end of the list
           Scientist *killMe = ptr->foo;   // another temporary pointer set to target (the next node)
       ptr->foo = killMe->foo;  // previous node is set to point to the node after the node to be deleted.
       delete killMe;
   }

And finally, let's write the destructor. We need it because the linked list is made up of dynamically allocated variables.

.. code-block:: c++

   BigBang::~BigBang()
   {
       Scientist *ptr = head;   // temporary pointer set to same node as head
       while (ptr != nullptr)
       {
           Scientist *ptr2 = ptr->foo;  // another temporary pointer set to the node after ptr
           delete ptr;  // the node that ptr points to
           ptr = ptr2;   // ptr now points to the next node
       }
   }   

Notice here that I am using two pointers to destruct a list.
Why?
If you try to destruct a node with one pointer, you can, but when you try to go to the next node, uh oh!
You've deleted the pointer that takes you to the next node. So what you need to do is have another pointer to act like a bookmark, ptr2 in this case.
Place ptr2 at the node after the one you want to delete, so when you want to get to that node, you can get its address from ptr2.

So, is a linked list better than an array? It's certainly more complicated. However, the linked list data structure allows you to insert, rearrange, and delete a node with more efficiency and speed (overall). However, it lacks the instant access an array gives you.
Let's see how an array can perform the actions we defined for a linked list by using an array of type BigBang.

ADDING AN ITEM TO THE FRONT

.. code-block:: c++

   int size = 10;
   BigBang scientists[size];
   for (int i = size - 1; i >= 0; i++)
        scientists[i] = scientists[i - 1];
   scientists[0].name = "Sheldon Cooper"; 
   scientists[0].field = "Theoretical physics";

ADDING AN ITEM TO THE REAR

.. code-block:: c++

   scientists[n - 1].name = "Amy Farrah Fowler";
   scientists[n - 1].field = "Neurobiology";

DELETING AN ITEM

.. code-block:: c++

   string name;
   cin >> name;
   for (int i = 0; i < size; i++)  // Less efficient and slower than linked list
       if (scientists[i].name== name)
      {
            for (int j = 0; j < size; j++)
                scientists[j] = scientists[j + 1];
            break;
      }

FINDING AN ITEM

.. code-block:: c++

   string name;
   cin >> name;
   for (int i = 0; i < size; i++)
       if (scientists[i].name == name)
            return true;

PRINTING ALL ITEMS

.. code-block:: c++

   for (int i = 0; i < size; i++)
       cout << "Name: " << scientists[i].name << "Field: "<< scientists[i].field << endl;

Doubly-linked Lists
-------------------

A doubly-linked list allows you to traverse a linked list both ways (like a two-way street!).
How? By adding another pointer to every node that points to the previous node.
This is a lot more complicated because you need to update three sets of pointers, in this order:
1) The new node's next and previous pointers.
2) The previous node's next pointer.
3) The following node's previous pointer.
Here's an example of what a node's stuct will now look like:

.. code-block:: c++

   struct Scientist
   {
        string name;
        string field;
        Scientist* foo;     // foo will point to the next Scientist item
        Scientist* bar;     // bar will point to the previous Scientist item
   };

By the way, the first node's bar pointer will be set to nullptr, not the head, to indicate the beginning of the array.