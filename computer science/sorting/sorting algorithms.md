# Sorting Algorithms

- [Sorting Algorithms](#sorting-algorithms)
  - [Initialization](#initialization)
  - [Bubble sort (15 sec)](#bubble-sort-15-sec)
  - [Selection sort (4 sec)](#selection-sort-4-sec)

## Initialization

Array size: **100 000 elements**.

## Bubble sort (15 sec)

Each pass of bubble sort steps through the list to be sorted compares each pair of adjacent items and swaps them if they are in the wrong order. At the end of each pass, the next largest element will “Bubble” up to its correct position. These passes through the list are repeated until no swaps are needed, which indicates that the list is sorted. In the worst-case, we might end up making an `n - 1` pass, where `n` is the input size.

<img src="https://upload.wikimedia.org/wikipedia/commons/c/c8/Bubble-sort-example-300px.gif">

Following is the implementation of the bubble sort algorithm in C#. The implementation can be easily optimized by stopping the algorithm when the inner loop didn't do any swap:

```csharp
public void Sort(int[] array)
{
    for (int i = 0; i < array.Length; i++)
    {
        for (int j = 1; j < array.Length - i; j++)
        {
            if (array[j - 1] > array[j])
                (array[j - 1], array[j]) = (array[j], array[j - 1]);
        }
    }
}
```

## Selection sort (4 sec)

The idea is to divide the array into two subsets —– sorted subset and unsorted subset. Initially, the sorted subset is empty, and the unsorted subset is the entire input list. The algorithm proceeds by finding the smallest (or largest, depending on sorting order) element in the unsorted subset, swapping it with the leftmost unsorted element (putting it in sorted subset), and moving the subset boundaries one element to the right.

Visual demonstration:

<img src="selection%20sort.png" width="200px">

C# implementation:

```csharp
public void Sort(int[] array)
{
    for (int i = 0; i < array.Length; i++)
    {
        int minIndex = i;
        for (int j = i + 1; j < array.Length; j++)
        {
            if (array[j] < array[minIndex])
                minIndex = j;
        }

        (array[i], array[minIndex]) = (array[minIndex], array[i]);
    }
}
```
