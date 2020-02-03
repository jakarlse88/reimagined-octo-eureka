# In-Place Algorithm
An _in-place_ method/function modifies data structures or objects outside of its own stack frame; as such, changes made by the method/function remain after the call completes. In-place algorithms are sometimes called _destructive_, since the original input is destroyed or at least modified during the method/function call.

(Note: in general, an in-place method/function will (only) create additional variables that are _O(1)_ space.)

An _out-of-place_ method/function doesn't mutate the original data structure; rather, it first copies the data structure before mutating it.

In many languages, _primitive values_ (`int`, `float`, `char`) are _passed by value_, ie. copied when passed as arguments. More complex data structures (arrays, heaps, stacks, hash tables, lists, dictionaries, etc.) are _passed by reference_, and as such they can be modified in place.

Examples of both an in-place and out-of-place method:
```C#
public void SquareArrayInPlace(int[] intArray)
{
    for (int i = 0; i < inArray.Length; i++)
    {
        intArray[i] *= intArray[i];
    }
}

public int[] SquareArrayOutOfPlace(int[] intArray)
{
    var squaredArr = new int[intArray.Length];

    for (int i = 0; i < intArray.Length; i++)
    {
        squaredArray[i] = intArray[i] * intArray[i];
    }

    return squaredArr;
}
```

Working in-place saves time and space: as an in-place algorithms doesn't initialise or copy data, it'll usually have a _O(1)_ space cost. However, as input is mutated there may be unintended side-effects that could have global effects. As such, out-of-place algorithms are considered safer and should be preferred unless space-constrained.