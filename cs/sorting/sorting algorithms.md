# Sorting algorithms

- [Sorting algorithms](#sorting-algorithms)
  - [Selection sort](#selection-sort)

```csharp
public interface ISorter
{
    void Sort(int[] array);
}
```

## Selection sort

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
