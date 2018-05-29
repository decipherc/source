.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Header Files
=============

A header file is where you should keep your class declaration(s).
The cpp file is where you should define them.
The sourse (cpp) and header files can be named whatever you want, regardless of what the class is called.
A cpp file needs to include its header file.
This basically copies and pastes the code from the header file into the cpp file, so you can use the information.
This is how you include a header called "Foo.h" in a cpp file.

.. code-block:: c++

   #include "Foo.h"

Notice that you use quotation marks "", not angle brackets <>, when including a header file. The same capitalization is maintained in both cases. Also notice that you do NOT put a semicolon after it.
You should NEVER include a cpp file in a header file or in another cpp file. You can only include header files, and they can only be included in cpp files. If you try and do otherwise you will get an error.
Also don't put "using namespace" commands in a header file (this is called namespace pollution). I don't think this will cause an error, but it's still bad to do. Put them in your cpp file instead.

// BAD!!!

Foo.h

.. code-block:: c++

      #include <iostream>
      using namespace std;

Foo.cpp

.. code-block:: c++

   #include "Foo.h"

// Good
Foo.h

.. code-block:: c++

   #include <iostream>

Foo.cpp

.. code-block:: c++

   #include "Foo.h"
   using namespace std;

So, "what do I do when I want to use things from the std?" you ask.
Simply put ``"std::"`` in front of them.
You do this any time you have ``cout``, ``cin``, ``endl``, or ``string``.

Put all header files a certain cpp needs in the cpp!
That is, don't assume a header file will be included in another one.
It will still work, but if you change your code in your cpp or header files, your header file not be including the other one any more.

You need to know when to include a header file.
If you think the answer is always, you're wrong!
The header file contains the full definition of the class, but you DO NOT need the full definition when...

1) You have a parameter of a function whose type is that class

.. code-block:: c++

   void bar(Foo x);

2) You have a function whose return type is that class

.. code-block:: c++

   Foo bar(int y);

3) You have a pointer or reference variable whose type is that class

.. code-block:: c++

   Foo *ptr;
   Foo &x;

So why do we even care?
You could include the header file in these cases and it would still work.
It's because
(a) header files can be really big and it would slow your compilation time unnecessarily and
(b) if two classes refer to each other's header files and both try to include each other header files, you will get a compiler error.

You will also get a compiler error if you include a header file more than once. Why? This means you are copying a class definition multiple times in the cpp file, and the compiler doesn't know which one to use (despite them being the same)! Even if you don't literally write the include statement twice in the cpp file, you can still make this mistake. How?
Say you have this situation, where "bar.h" is being defined three times:

main.cpp

.. code-block:: c++

   #include "foo.h"
   #include "bar.h"

foo.cpp

.. code-block:: c++

   #include "foo.h"

foo.h

.. code-block:: c++

   #include "bar.h"

bar.cpp

.. code-block:: c++

   #include "bar.h"

There is a way to prevent this. You can use include guards around all of your headers. An include guard checks whether you have already defined a header before allowing the compiler to execute the code.

.. code-block:: c++

   foo.h
   #ifndef FOO_H        // If foo.h has not been defined...
   #define FOO_H       // ...define it
   #include "bar.h"
   ................
   #endif                     // end of the include guard (for foo.h)

   bar.cpp
   #ifndef BAR_H        // If bar.h has not been defined...
   #define BAR_H       // ...define it
   #include "bar.h"
   ................
   #endif                     // end of the include guard (for bar.h)

If the header as NOT been defined, only the code between the #ifndef and #endif will compile.
Notice that when using #ifndef and #define, the header name has an underscore instead of a period and is written in all capital letters.
There is also #ifdef and #endif, but we won't be using them.
#define is used to define new constants in general, but we won't be using it this way.
Include guards should always be used, regardless of they are actually necessary, as safe practice.
