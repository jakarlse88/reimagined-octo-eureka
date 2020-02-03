### Big O
The language we use to reason about how long an algorithm takes to run. We express the runtime in terms of how quickly it grows relative to the input, as the input grows arbitrarily large. We express this “speed” using the size of the input _n_, and we can for example say that “the runtime grows on the order of the square of the input”, which would be _O(n<sup>2</sup>)_.

Some examples:
- _O(1)_ algorithms run in constant time relative to their input—regardless of the size of the input, the function would only require a single step
- _O(n)_ algorithms run in linear time: if the input n is of size 100, the function will perform 100 steps
- _O(n<sup>2</sup>)_ algorithms run in quadratic time: if the input n is of size 100, the function will perform 10,000 steps

Note: _n_ can be either the size of the input, or the actual input.

### Constants
```C#
  public void PrintFirstItemThenFirstHalfThenSayHi100Times(int[] items)
{
    Console.WriteLine(items[0]);

    int middleIndex = items.Length / 2;
    int index = 0;

    while (index < middleIndex)
    {
        Console.WriteLine(items[index]);
        index++;
    }

    for (int i = 0; i < 100; i++)
    {
        Console.WriteLine("hi");
    }
}
```

This function is strictly speaking _O(1 + n/2 + 100)_, but we call it _O(n)_. As _n_ gets arbitrarily large, adding 100 or dividing by 2 has a decreasingly significant effect. 

### Less significant terms
Similarly, the runtime _O(n + n2)_ is just called _O(n2)_--and it would still be called thus even if it was _O(n2 / 2 +100n)_. 
- _O(n3+50n2+10000)_ is _O(n<sup>3</sup>)_
- _O((n+30)∗(n+5))_ is _O(n<sup>2</sup>)_

As _n_ gets arbitrarily large, the less significant terms become increasingly less important (as the name implies).

### Worst case
The “worst case” stipulation is mostly implied when talking Big O, so when talking about a best case especially it pays to be explicit. Further, for some algorithms we can make rigorous statements about the “average case” runtime. 

```C#
public bool Contains(int[] haystack, int needle)
{
    // Does the haystack contain the needle?
    foreach (var n in haystack)
    {
        if (n == needle)
        {
            return true;
        }
    }
    return false;
}
```

This algorithm has a best case of _O(1)_, and a worst case of _O(n)_, so usually we'd just say it runs in _O(n)_.

### Space complexity
Reasoning about space complexity/memory cost is similar to reasoning about time cost; look at the total size, relative to the size of the input, of any new variables being allocated.

_O(1)_:
```C#
public void SayHiNTimes(int n)
{
    for (int i = 0; i < n; i++)
    {
        Console.WriteLine("hi");
    }
}
```

_O(n)_:
```C#
public string[] ArrayOfHiNTimes(int n)
{
    string[] hiArray = new string[n];
    for (int i = 0; i < n; i++)
    {
        hiArray[i] = "hi";
    }
    return hiArray;
}
```

**Usually when reasoning about space complexity, we mean _additional space_**. Thus we don't include space taken up by inputs.

```C#
public int GetLargestItem(int[] items)
{
    int largest = int.MinValue;
    foreach (int item in items)
    {
        if (item > largest)
        {
            largest = item;
        }
    }
    return largest;
}
```
This last method is also _O(n)_ (constant space).

### Caveats
"Develop the skill to see time and space optimisations, as well as the wisdom to judge if those optimisations are worthwhile."
