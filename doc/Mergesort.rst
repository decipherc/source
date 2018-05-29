.. decipher documentation master file, created by
   sphinx-quickstart on Thu Feb  5 18:25:10 2015.
   You can adapt this file completely to your liking, but it should at least
   Inheritance.rst
   contain the root `toctree` directive.

Merge Sort
==========

Merge sort breaks the array into halves and merges them together.

1. Check that the array has at least two elements
2. Split up the array in halves
3. Recursively repeat step 1 - 2 until you have at least two elements
4. Merge the two halves.

2 6 5 1 4 3
Half 1: 2 6 5
Half 1.1: 2
Half 1.2: 6 5
Half 1.2.1: 6
Half 1.2.2: 5
Merge half 1.2.1 and 1.2.2
5 6
Merge half 1.1 and 1.2
2 5 6
Half 2: 1 4 3
Half 2.1: 1
Half 2.2: 4 3
Half 2.2.1: 4
Half 2.2.2: 3
Merge half 2.2.1 and 2.2.2
3 4
Merge half 2.1 and 2.2
1 3 4
Merge half 1 and 2
1 2 3 4 5 6

.. code-block:: c++

   void mergeSort(int foo[], int n)
   {
        if (n <= 1)
             return;
        mergeSort(foo, n / 2);
        mergeSort(foo + n / 2, n);
        merge(foo, n / 2, n);     // foo now sorted
   }


   void merge(int arr[], int n1, int n2)
   {
        int i = 0, j = 0, k = 0;     // i is used for first half of inputted array, j for second half, k for temp array
        int *temp = new int[n1 + n2];     // temporary array
        int *first = array;          // Pointer to beginning of first half
        int *second = first + n1;     // Pointer to beginning of second half

        while (i < n1 || j < n2)
        {
             if (i == n1)     // first reached the end of its half
                  temp[k++] = second[j++];

             else if (j == n2)     // second reached the end of its half
                  temp[k++] = first[i++];

             else if (first[i] < second[j])
                  temp[k++] = first[i++];     // Copy the value of the smaller number into temp, then increment the variables

             else // The second number must be smaller or equal to the first number
                  temp[k++] = second[j++];
        }
        for (i = 0; i < n1 + n2; i++)
             arr[i] = temp[i];     // Override the contents of the original array with temp
        delete [] temp;
   }

The big-o is always O(nlog(n)). Why? The mergeSort function always breaks the array in half log(n) times, and the merge function merges n items every time.

.. code-block:: c++

   void mergeSort(int foo[], int n)
   {
        if (n <= 1)
             return;
        mergeSort(foo, n / 2);     // O(log(n))
        mergeSort(foo + n / 2, n);     // O(log(n))
        merge(foo, n / 2, n);     // O(n)
   }     // O(nlog(n))


   void merge(int arr[], int n1, int n2)
   {
        int i = 0, j = 0, k = 0;
        int *temp = new int[n1 + n2];
        int *first = array;
        int *second = first + n1;

        while (i < n1 || j < n2)     O(n)
        {
             if (i == n1)
                  temp[k++] = second[j++];

             else if (j == n2)
                  temp[k++] = first[i++];

             else if (first[i] < second[j])     // ** Should this be <= instead?
                  temp[k++] = first[i++];

             else
                  temp[k++] = second[j++];
        }
        for (i = 0; i < n1 + n2; i++)     O(n)
             arr[i] = temp[i];
        delete [] temp;
   }     O(n) (f(n) = n + n = 2n)

The merge sort is stable.

2(a) 2(b) 1
Half 1: 2(a)
Half 2: 2(b) 1
Half 2.1: 2(b)
Half 2.2: 1
Merge 2.1 and 2.2
1 2(b)
Merge 1 and 2
2(a) 1 2(b)
n1 = 1
n2 = 2
i = 0 (2(a))     index(value)
j = 0 (1)
first[0] > second[0], copy 1
temp = 1
j = 1 (2(b))
first[0] == second[1], copy 2(b)     ** Is this a mistake? Wikipedia says that mergesort is stable most of the time, is this an exception or is there a mistake?
temp = 1 2(b)
j = 2 (?)
j == n2, copy 2(a)
temp = 1 2(b) 2(a)
arr = 1 2(b) 2(a)
