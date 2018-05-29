.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

===========
Inheritance
===========

Inheritance is a technique used in C++ that allows you to reuse code in one class or object (called a base class)
in another (called a derived class) without actually copying everything; you are inheriting information and methods.
How convenient!
When would you use this?
Say you have a person object.
Let's say a person has a name, a job, a way to get and set the name, and a way to get and set the job title.
Now let's create a wizard object.
A wizard has a wand, in addition to a name and a job.
We can have the wizard object inherit the name and job features.
But why don't we just copy and paste it? In coding having duplicate code is considered bad.
Why?
If you change one of the versions of the code, you may incorrectly copy the changes to the other versions or even forget to completely.
This can result in careless mistakes.
Also it takes time! 

VERSION WITHOUT INHERITANCE

.. code-block:: c++

   class Person
   {
   public:
        void setName(string nm){
             m_name = nm;     
        }
        string getName(){
             return m_name;
        }
        void setJob(string jb){
                  m_job = jb;
                  party();
        }
        string getJob(){
             return getJob;
        }
   private:
        void party(){
             cout << "Wooo!" << endl;
        }
        string m_name;
        string m_job;
   };
   
   class Wizard
   {
   public:
        void setName(string nm){
             m_name = nm;     
        }
        string getName(){
             return m_name;
        }
        void setJob(string jb){
                  m_job = jb;
                  party();
        }
        string getJob(){
             return getJob;
        }
        void setWand(string w){
             m_wand = w;
        }
        string getWand(){
             return m_wand;
        }
   private:
        void party(){
             cout << "Wooo!" << endl;
        }
        string m_name;
        string m_job;
        string m_wand;
   };

Notice that setName, getName, setJob, getJob, and party are the same in both classes.
To have wizard inherit these functions, put, after the name of the class, a colon, write "public", and then the name of the class from which you want to get the functions from.
Now you delete the duplicate functions and data members from Wizard, and you will still be able to use them as if they were there.
How?
When you write ": public Person", the compiler automatically copies and pastes all of Person's public functions into Wizard's definition.
However, you still can't access the base class' private members.
As a professor of mine once said, you can't touch your parent's privates; only other instances of the same class (or other Wizards) may touch your privates. 

VERSION 1 WITH INHERITANCE

.. code-block:: c++

   class Person
   {
   public:
        void setName(string nm){
             m_name = nm;     
        }
        string getName(){
             return m_name;
        }
        void setJob(string jb){
                  m_job = jb;
                  party();
        }
        string getJob(){
             return getJob;
        }
   private:
        void party(){
             cout << "Wooo!" << endl;
        }
        string m_name;
        string m_job;
   };
   
   class Wizard : public Person
   {
   public:
        void setWand(string w){
             m_wand = w;
        }
        string getWand(){
             return m_wand;
        }
   private:
        void party(){
             cout << "Wooo!" << endl;
        }
        string m_wand;
   };
   
   int main()
   {
        Person bob;
        bob.setName("Bob Bobington");
        bob.setJob("Bobber");
        bob.getName();
        bob.getJob();
   
        Wizard harry;
        harry.setName("Harry Potter");     // Uses class Person
        harry.setJob("Auror");
        harry.getName();
        harry.getJob();     
        harry.setWand("dragon heart");
        harry.getWand();     
   
        bob.setWand();     // Error! Person can't access Wizard's functions! It doesn't even know Wizard exists.
   
        bob.party();     // Error! Can't access a class' private functions from main
        harry.party();     // Error! Can't access a base class' private functions, anywhere, period.
   }
    
But what about Person's private function ``party()`` that we also want to have in Wizard?
Note that you CANNOT call any of Person's private data, whether they be functions or variables (or any class' privates that aren't yours, for that matter). So what do we do? C++ provides an option in which you can declare a protected function. A protected function can only be accessed by the derived class. This allows the derived class' public functions to use it, but that's it. You still can't access it in main or anywhere else.

VERSION 2 WITH INHERITANCE

.. code-block:: c++

   class Person
   {
   public:
        void setName(string nm){
             m_name = nm;     
        }
        string getName(){
             return m_name;
        }
        void setJob(string jb){
                  m_job = jb;
                  party();
        }
        string getJob(){
             return getJob;
        }
   protected:
        void party(){
             cout << "Wooo!" << endl;
        }
   private:
        string m_name;
        string m_job;
   };
   
   class Wizard : public Person
   {
   public:
        void setWand(string w){
             m_wand = w;
        }
        string getWand(){
             return m_wand;
        }
   private:
        string m_wand;     
   };
   
   int main()
   {
        Person bob;
        bob.setName("Bob Bobington");
        bob.setJob("Bobber");
        bob.getName();
        bob.getJob();
   
        Wizard harry;
        harry.setName("Harry Potter");
        harry.setJob("Auror");
        harry.getName();
        harry.getJob();     
        harry.setWand("dragon heart");
        harry.getWand();     
   
        bob.setWand();     // Error! Person can't access Wizard's functions! It doesn't even know Wizard exists.
   
        bob.party();     // Error! Can't access a class' private functions from main
        harry.party();     // Error! Can't access a class' private functions from main
   }

There are three types of inheritance: reuse, extension, and specialization. Don't worry about these formal names; they are just words to describe different things you can do with inheritance.

Reuse: Reuse is when you use inheritance to duplicate the base class' methods in the derived class.

.. code-block:: c++

   class Person
   {
   public:
        void setName(string nm){
             m_name = nm;     
        }
        string getName(){
             return m_name;
        }
        void setJob(string jb){
                  m_job = jb;
                  party();
        }
        string getJob(){
             return getJob;
        }
   protected:
        void party(){
             cout << "Wooo!" << endl;
        }
   private:
        string m_name;
        string m_job;
   };
   
   class Muggle : public Person     // Muggle can do all of the same things a Person can, and no more
   {
   };
   
   int main()
   {
        Person bob;
        bob.setName("Bob Bobington");
        bob.setJob("Bobber");
        bob.getName();
        bob.getJob();
   
        Muggle jane;
        jane.setName("Jane Janeson");
        jane.setJob("Jailer");
        jane.getName();
        jane.getJob();
   }

Extension: Extension is when you use inheritance to add methods in addition to the base class' methods.
This is what we did in the example above; we added two new public functions to the derived class Wizard.

Specialization: Specialization is when you use inheritance to change the base class' methods in the derived class.
When doing this you need to include the word "virtual" in front of all functions you plan to change in both the based and derived class;
this let's the compiler know that there is a different version in a derived class.
We'll go over in more detail what this means later. Note that you do NOT write virtual in front of the function's name if you define it outside of its class declaration;
you only use virtual in the function's definition or in class declaration.
Say we want to change Wizard's party function to say something else (I'm going to change it to be public as well so we can call it directly from main).
First we need to add "virtual" to the base class' party function.
Then we add our new party function to the derived class and add "virtual" to the beginning.

.. code-block:: c++

   class Person
   {
   public:
        void setName(string nm){
             m_name = nm;     
        }
        string getName(){
             return m_name;
        }
        void setJob(string jb){
                  m_job = jb;
                  party();
        }
        string getJob(){
             return getJob;
        }
        virtual void party(){
             cout << "Wooo!" << endl;
        }
   private:
        string m_name;
        string m_job;
   };
   
   class Wizard : public Person
   {
   public:
        void setWand(string w){
             m_wand = w;
        }
        string getWand(){
             return m_wand;
        }
        virtual void party(){
             cout << "Wooo, I'm a wizard!" << endl;
        }
   private:
        string m_wand;     
   };  

Now when you call the party function for harry, an instance of the Wizard class, it will print out "Wooo, I'm a wizard!".

.. code-block:: c++

   int main()
   {
        Wizard harry;
        harry.party();     // Prints out "Wooo, I'm a wizard!"
   }

Let's go over how the compiler works in this case.
When you call the party function, the compiler first goes to Person's party function (``Person::party()``).
When it sees that it has the word "virtual" in front of it, he thinks, "oh wait, I need to use the one defined by the most derived class," in this case,
Wizard (``Wizard::party()``). 

If you do want to call the Person's party function with harry, you would do so with:

.. code-block:: c++

   harry.Person::party();

When constructing a derived class, whose constructor gets called first, the base class or the derived class?
The base class, because it needs to be constructed before you try and use it (which you can in the derived class).
And for destructors, the derived class gets destructed first.
If the base class needs parameters for its constructor (i.e. it is not a default constructor),
you need to initialize it with a initializer list, in the form of className(argument).

.. code-block:: c++

   class Person
   {
   public:
        Person(int age){
             m_age = age;
        }
        virtual void setName(string nm){
             m_name = nm;     
        }
        string getName(){
             return m_name;
        }
        virtual void party(){
             cout << "Wooo!" << endl;
        }
   private:
        string m_name;
        int m_age;
   };
   
   class Wizard : public Person
   {
   public:
        Wizard(int age) : Person(age) {
             m_wand = "";
        }
        void setWand(string w){
             m_wand = w;
        }
        string getWand(){
             return m_wand;
        }
        virtual void party(){
             cout << "Wooo, I'm a wizard!" << endl;
        }
   private:
        string m_wand;     
   };  
   
   int main()
   {
        Wizard harry(17);
   }