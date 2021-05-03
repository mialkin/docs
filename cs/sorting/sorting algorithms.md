# Sorting algorithms

- [Sorting algorithms](#sorting-algorithms)
  - [Naive sort](#naive-sort)
  - [Bubble sort](#bubble-sort)
  - [Selection sort](#selection-sort)

Array size: **100 000 elements**.

Array generation algorithm:

```csharp
int size = 100000;

int[] arr = Generate();

int[] Generate()
{
    int[] array = new int[size];

    Random randNum = new();
    for (int i = 0; i < size; i++)
    {
        array[i] = randNum.Next(0, size);
    }

    return array;
}
```

Protocol for a sorter:

```csharp
public interface ISorter
{
    void Sort(int[] array);
}
```

## Naive sort

Sort time: **19s**.

Swaps two elements if the second is smaller than the first. Does so until there are no swaps anymore.

```csharp
public class NaiveSorter : ISorter
{
    public void Sort(int[] array)
    {
        bool swapped = true;

        while (swapped)
        {
            swapped = false;

            for (int i = 0; i < array.Length - 1; i++)
            {
                if (array[i] > array[i + 1])
                {
                    int temp = array[i];
                    array[i] = array[i + 1];
                    array[i + 1] = temp;

                    swapped = true;
                }
            }
        }
    }
}
```

## Bubble sort

Sort time: **15s**.

```csharp
public class BubbleSorter : ISorter
{
    public void Sort(int[] array)
    {
        for (int j = array.Length - 1; j > 0; j--)
        {
            for (int i = 0; i < j; i++)
            {
                if (array[i] > array[i + 1])
                {
                    int temp = array[i];
                    array[i] = array[i + 1];
                    array[i + 1] = temp;
                }
            }
        }
    }
}
```

<img src="https://upload.wikimedia.org/wikipedia/commons/c/c8/Bubble-sort-example-300px.gif">

## Selection sort

Sort time: **4s**

```csharp
public class SelectionSorter : ISorter
{
    public void Sort(int[] array)
    {
        for (int i = 0; i < array.Length; i++)
        {
            var min = array[i];
            var minIndex = i;

            for (int j = i; j < array.Length; j++)
            {
                if (array[j] < min)
                {
                    min = array[j];
                    minIndex = j;
                }
            }

            array[minIndex] = array[i];
            array[i] = min;
        }
    }
}
```

<img src="https://upload.wikimedia.org/wikipedia/commons/9/94/Selection-Sort-Animation.gif">
