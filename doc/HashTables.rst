.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Hash Tables
===========

Before I start with hash tables, first I'll discuss some of the properties of the modulus operator.
The modulus operator divides two numbers and gets the remainder.

Example:

.. code-block:: c++

   1234 % 100 is 34

If you use the modulus operator to divide numbers by a certain number ``N``, like ``5``, you will get a pattern of numbers that never go higher than ``N-1``.

.. code-block:: c++

   N = 5
   0 % 5 = 0
   1 % 5 = 1
   2 % 5 = 2
   3 % 5 = 3
   4 % 5 = 4
   5 % 5 = 0
   6 % 5 = 1

.. code-block:: c++

   N = 2
   0 % 2 = 0
   1 % 2 = 1
   2 % 2 = 0
   3 % 2 = 1

A hash table is a type of ADT that is very efficient for inserting and searching for data.
It typically uses an array for its data structure.

The algorithm is that you insert a value into the array based on a specific position determined by a hash function.
Basically, almost all values inserted have a unique position based on their value, so you can calculate the position of the value based on a function and access it in one step!

Say you want to store all the students 9-digit IDs of students at UCLA.
One possibility is to create an array of ``100,000,000`` items and store every value in a bucket (position) based on its literal value.

Example:

ID = 123456789 is stored in ``arr[123456789]`` (By the way, this array would be of type bool, ``true == someone`` has that id, false they don't)

.. code-block:: c++

   int hashFunc(int id)
   {
        int bucket = id;
        return bucket;
   }

This will be really efficient, but it will also waste a LOT of memory (there are only 50,000 students at UCLA).
Why don't we use the modulus operator to get the first four digits of a student id for the bucket number, and then just have an array of 100,000 elements.

Example:

ID = 123456789 is stored in arr[56789]

.. code-block:: c++

   int hashFunc(int id)
   {
        int bucket = id % 100000; // This guarantees us we will get a bucket number between 0 and 99,999
        return bucket;
   }

This looks pretty good, but what if we want to put in an id of 111156789?
Our hash function will return a bucket of 56789, but that spots already taken!
This is called a collision: when two or more values both hash to the same bucket in the array.

There are two ways of dealing with collisions: by creating a Closed Hash Table or an Open Hash Table.

Closed Hash Table
-----------------

The closed hash table uses a linear probing algorithm, which is basically a gross way of saying that you put each value as close as possible to its intended bucket.

.. code-block:: c++

   [56789]     123456789     // We can't put our id here, let's try the next spot
   [56790]     111156789     // This is free, let's just put it here

But the problem with the closed hash table is that once you run out of empty buckets, you can't add any new values and you can't delete any values either.

Here's the code:

In this example we will have an array of 5 phone numbers (for all of your friends).

Every bucket in a closed hash table is actually a struct with a value and a bool for whether it has been used or not.

.. code-block:: c++

   struct bucket
   {
        int num;
        bool used;
   }

Let's create a class, a data structure, and some interface functions.

.. code-block:: c++

   class HashTable
   {
   public:
        void insert(int num);
        bool search(int num);
   private:
        int hashFunc(int num) const     // Let's keep our secret hash function secret
        {
             int bucket = num % 5;     // 5 is the number of total buckets
             return bucket;
        }
        bucket m_phoneBook[5];
   };
   
   void HashTable::insert(int num)
   {
        int bucket = hashFunc(num);
   
        for (int tries = 0; tries < 5; tries++)
        {
             if (m_phoneBook[bucket].used == false)     // If the hashed value is free
             {
                  m_phoneBook[bucket].num = num;
                  m_phoneBook[bucket].used = true;
                  return;
             }
             // Linear probing
             bucket = (bucket + 1) % 5;     // Advance to the next bucket. Use this rather than bucket++ because it will go from 9 to 0 and wrap around
         }
        // If we get here, there isn't any room in the table, so we can't do anything
   }
   
   
   bool HashTable::search(int num)
   {
        int bucket = hashFunc(num);
   
        for (int tries = 0; tries < 5; tries++)
        {
            if (m_phoneBook[bucket].used == false)     // If nothing is in the hashed value, it can't be there
   
                 return false;
   
            if (m_phoneBook[bucket].num == num)     // If it is in the hashed value, yay we found it
   
                 return true;
   
   
             // Linear probing
             bucket = (bucket + 1) % 5;     // Advance to the next bucket. Use this rather than bucket++ because it will go from 9 to 0 and wrap around
        }
        return false;     // Must not be in the table
   }

Open Hash Table
---------------

Here is the algorithm to insert an item:
1. Compute a bucket number with your hash function
2. Add your new value to the linked list at array[bucket]
3. Done!

To search for an item:
1. Compute a bucket number with your hash function
2. Search the linked list at array[bucket] for your item
3. If you reach the end of the linked list, it's not in the table

.. code-block:: c++

   struct bucket
   {
        int num;
        bool used;
        bucket* next;
   }
   
   class HashTable
   {
   public:
        void insert(int num);
        bool search(int num);
        void delete(int num);
   private:
        int hashFunc(int num) const     // Let's keep our secret hash function secret
        {
             int bucket = num % 5;     // 5 is the number of total buckets
             return bucket;
        }
        bucket* m_phoneBook[5];     // Array of bucket pointers
   };
   
   void HashTable::insert(int num)
   {
        int bucket = hashFunc(num);
   
        bucket* traverser = phoneBook[bucket];
   
        while (traverser->next != nullptr)
             traverser = traverser->next;
   
        bucket* b = new bucket;
        b->num = num;
        b->used = true;
        b->next = nullptr;
   
        traverser->next = b;
   }
   
   bool HashTable::search(int num)
   {
        int bucket = hashFunc(num);
   
        bucket* seeker = phoneBook[bucket];
   
        while (seeker->next != nullptr)
        {
             if (seeker->num == num)
                  return true;
             seeker = seeker->next;
        }
        return false;
   }

.. code-block:: c++

   void HashTable::delete(int num)
   {
        int bucket = hashFunc(num);
   
        bucket* head = phoneBook[bucket];
   
        if (head == nullptr)
             return;
        if (head->num == num)
        {
             bucket* victim == head;
             head = victim->next;
             delete victim;
             return;
        }
   
        bucket *p = head;
        while (p != nullptr)
        {
             if (p->next != nullptr && p->next->num == num)
                  break;
             p = p->next;
        }
        if (p != nullptr)
        {
             bucket *victim = p->next;
             p->next = victim->next;
             delete victim;
        }
   }

Calculating the hash table efficiency

First calculate the load factor:

.. code-block:: c++

   L = (max number of values you plan to insert) / (total buckets in the array)

A load of ``0.1`` means you will only fill ``0.1*100 = 10%`` of your array

For a closed hash table, here's how to compute the average number of tries it will take you to insert or find an item (if your ``L`` is less than ``1``)

Average number of tries = ``(1/2)(1 + 1 / (1 - L))``

For a open hash table, here's how to compute the average number of tries it will take you to insert or find an item

Average number of tries = ``1 + (L / 2)``

To decide how big you should make your array (for an open hash), set the average number of tries equal to ``1.25`` and solve for ``L``.

.. code-block:: c++

      1.25 = 1 + (L / 2)
      L = .5

Plug in the value for ``L`` and how many values you want to insert and solve.

.. code-block:: c++

   L = (max number of values you plan to insert) / (total buckets in the array)

.. code-block:: c++

   0.5 = (max number of values you plan to insert) / (total buckets in the array)