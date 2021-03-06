---
title: insertion and merge sort
date: 2018-04-01
---

#Insertion and Merge Sort

Sorting an array is one of the cornerstone problems in computer science. In this post, we explore some algorithms for this task. All the code in this post can [be](../../CALGO/sorting/insertion_merge_sort.c) [found](../../CALGO/sorting/insertion_merge_sort_unboxed.c) [here](../../CALGO/sorting/sort_test.c). The algorithms are reproduced below. The linked directory also contains some code for testing the algorithms.

### Insertion Sort

Insertion sort begins with an array $A$. Suppose that $A$ is sorted up to index $i$. If we insert $A_{i+1}$ into the correct place, then $A$ will be sorted up to index $i+1$. This allows us to inductively sort the array. Here is a C procedure which does this:

```{.c}
void insertion_sort ( int *array, int length )

/*

  array is a pointer to the first element of the array
  length is the length of the array

*/

{
  int j;          /* index of the next element to be inserted */
  int i;          /* index for looping through the sorted part of array */
  int element;    /* next element to be inserted */

  for (j = 1; j < length; j++)
    {
      element = array[j];      /* store the element to be inserted */
      i = j - 1;               /* store the top index of the already sorted part */
      while ( (i >= 0) && (array[i] > element) )
        /*
          loop through the already sorted part
          searching for where element needs to be inserted.
          At the same time, we also move the parts bigger than element one
          spot to the right.
         */
        {
          array[i+1] = array[i];
          i = i - 1;
        }
      array[i+1] = element; /* slot element in */
    }
}
```
In terms of the length $L$ of the array, the space complexity of the algorithm is $O(1)$ and the time complexity is $O(L^2)$.


### Merge Sort

The merge sort algorithm recursively sorts the first half of the array, then recursively sorts the second half of the array and then merges the result together.

```{.c}
void merge ( int *array, int low, int middle, int high)
/*

  array is a pointer to the first element of the array.
  low, mid and high are indices in the array with low <= mid < high
  the procedure assumes that array[low,mid] and array[mid+1,high] are sorted.
  It merges them so that array[low,high] is sorted.

*/

{
  int l1 = middle - low + 1;   /* length of first subarray */
  int l2 = high - middle;      /* length of second subarray */


  /*
    allocating some temporary arrays on the stack.
    this is going to limit the size the array which this procedure can work with.
    If high - low is too large, we are going to overflow the stack.

    Also, L and R are one entry bigger than the corresponding sub arrays.
    This is because we put a very large token at the end to help us in the following computation.
   */
  int l[l1+1];
  int r[l2+1];

  int i;  /* index for L */
  int j;  /* index for R */
  int k;  /* index for array */


  /* initializing L */
  for (i = 0; i < l1; i++)
    l[i] = array[low+i];

  l[l1] = INT_MAX;

  /* initializing R */
  for (j = 0; j < l2; j++)
    r[j] = array[middle + 1 + j];

  r[l2] = INT_MAX;

  i = j = 0;

  for (k = low; k <= high; k++)
    {
      if (l[i] <= r[j])
        {
          array[k] = l[i];
          i = i + 1;
        }
      else
        {
          array[k] = r[j];
          j = j + 1;
        }
    }
}


void merge_sort_sub(int *array,int low,int high)
{
  int mid;

  if (low < high)
    {
      mid = (low + high) / 2;  /* integer division rounds down */
      merge_sort_sub(array,low,mid);
      merge_sort_sub(array,mid+1,high);
      merge(array,low,mid,high);
    }
}

void merge_sort(int *array, int length)
{

  /* arguments two and three for merge_sort_sub must be indices for the array */ 
  merge_sort_sub(array,0,length-1);
}
```
Let $T(L)$ be the run time of this algorithm. Then we have the relation
$$
T(L) = 2 T(L/2) + cn = 4 T(L/4) + 2 cn = \cdots \leq c' L \log_2 L.
$$
Therefore the time complexity of merge sort is $O(L \log L)$. The space complexity of this procedure is a little more subtle. Since the `merge` procedure allocates the arrays `l` and `r` on the stack, we just need to understand how the stack grows and shrinks during execution. Let $S(L)$ be the maximum stack size during execution. Then 
$$ S(L) \leq {\rm max} \{ S(L/2),cL \} $$
because `merge` allocates a whole copy of the array on the stack. 
This implies the stack grows by $O(L)$ during execution.

### Boxing things up

If you looked at the linked source code, you may have noticed that the procedures from the previous section are actually called `insertion_sort_unboxed` and `merge_sort_unboxed`. This is because they only work on lists of 32 bit integers. In practice, we want to sort more general types of arrays. We can achieve this by abstracting away the data which is contained in our array. This allows the algorithms to be used in more situations, but it also means they run slower than the unboxed versions. Before we can write generic versions of insertion and merge sort, we need to define a data structure that they will work on.

```{.c}
typedef struct
{
  /* array of pointers */
  void **ptr;

  /*
    we had to pass the array length to the unboxed procedures.
    Here we can wrap it up in the data structure.
   */
  int length;

  /*
    given two elements of the list, we need to compare them.
    (*compare)(ptr1,ptr2) returns 1 if *ptr1 < *ptr2
   */
  int (*compare)(void *ptr1, void *ptr2);
} list;
```
The variable `ptr` represents a lists of pointers, each pointing to some presumeably complex data structure. The `length` variable represents the length of the list and `compare` points to a procedure which lets us compare the elements of the list. In terms of this data structure, the insertion sort procedure is defined as follows:
```{.c}
void insertion_sort (list *l)
{
  int j;            /* index of the next element to be inserted */
  int i;            /* index for looping through the sorted part of array */
  void *element;    /* next element to be inserted */

  for (j = 1; j < l->length; j++)
    {
      element = l->ptr[j];     /* store the element to be inserted */
      i = j - 1;               /* store the top index of the already sorted part */
      while ( (i>= 0) && (*l->compare)(element, l->ptr[i]))
        /*
          loop through the already sorted part
          searching for where element needs to be inserted.
          At the same time, we also move the parts bigger than element one
          spot to the right.
         */
        {
          l->ptr[i+1] = l->ptr[i];
          i = i - 1;
        }
      l->ptr[i+1] = element;      /* slot element in */
    }
}
```
It is almost identical to the unboxed version. The generic merge sort procedure looks a little different. In the unboxed version, we used an infinity token during the `merge` procedure. There isn\'t an obvious way to do this in the generic case, so we need some extra logic.
```{.c}
void merge ( list *list, int low, int middle, int high)
/*

  list is a pointer to a list structure.
  low, mid and high are indices with low <= mid < high
  the procedure assumes that l->ptr[low,mid] and l->ptr[mid+1,high] are sorted.
  It merges them so that l->ptr[low,high] is sorted.

*/

{
  int l1 = middle - low + 1;   /* length of first subarray */
  int l2 = high - middle;      /* length of second subarray */


  /*
    allocating some temporary arrays on the stack.
    In the unboxed version, we used an infinity token.
    In this case we can't do that because the infinity token is different for each
    data type.
  */
  void *l[l1];
  void *r[l2];

  int i;  /* index for L */
  int j;  /* index for R */
  int k;  /* index for list */


  /* initializing L */
  for (i = 0; i < l1; i++)
    l[i] = list->ptr[low+i];


  /* initializing R */
  for (j = 0; j < l2; j++)
    r[j] = list->ptr[middle + 1 + j];

  i = j = 0;

  for (k = low; k <= high; k++)
    {
      if (i == l1 || j == l2)
        /* once we reach the end of either array, we break out of the loop */
        break;
      else
        {
          /* since the compare function computes <, we need to negate to get >= */
          if (! (*list->compare)(r[j],l[i]))
            {
              list->ptr[k] = l[i];
              i = i + 1;
            }
          else
            {
              list->ptr[k] = r[j];
              j = j + 1;
            }
        }
    }

  /* fill the list with the remaining elements */
  if (i == l1)
    {
      for (;k <= high; k++)
        {
          list->ptr[k] = r[j];
          j = j + 1;
        }
    }
  else
    {
      for (;k <= high; k++)
        {
          list->ptr[k] = l[i];
          i = i + 1;
        }
    }

}

void merge_sort_sub(list *l,int low,int high)
{
  int mid;

  if (low < high)
    {
      mid = (low + high) / 2;  /* integer division rounds down */
      merge_sort_sub(l,low,mid);
      merge_sort_sub(l,mid+1,high);
      merge(l,low,mid,high);
    }
}


void merge_sort(list *l)
{

  /* arguments two and three for merge_sort_sub must be indices for the array */
  merge_sort_sub(l,0,l->length-1);
}

```
The boxed versions of the algorithms have the same asymptotic space and time complexity as the unboxed versions. The constants hidden by the $O$ notation are larger in the boxed versions. In future posts, we shall try to write generic versions of algorithms.
