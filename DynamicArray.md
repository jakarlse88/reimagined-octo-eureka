# Dynamic Array
Also: _Array list, growable array, resizable array, mutable array_

A _dynamic array_ is an array with automatic resizing, expanding as more elements are added (eliminating the need to determine its size ahead of time). 

### Strengths:
- **Fast lookups:** like static arrays, retrieving the element at a given index takes _O(1)_ time
- **Variable size:** the dynamic array will expand to hold as many items as is necessary
- **Cache-friendly:** like arrays, dynamic arrays place elements right next to each other in memory, making efficient use of caches

### Weaknesses
- **Slow worst-case appends:** appending an element to a dynamic array usually takes _O(1)_ time, however if the dynamic array doesn't have room for the new item it'll need to expand (which takes _O(n)_ time)
- **Costly inserts and deletes:** like arrays, elements are stored sequentially, thus adding or removing an item in the middle of the dynamic array requires the shifting forward/backward of other elements (which takes _O(n)_ time)

In C#, `List` is a dynamic array implementation.

## Doubling Appends
If an element is appended to the dynamic array but it's already at capacity, the dynamic array automatically creates a new and bigger underlying static array--usually twice as big. Each existing element has to be copied over to the new array, which takes _O(n)_ time. This describes the worst case--in the best and average case, appends are _O(1)_ time.

## Amortised Cost of Appending
1. The time cost of each doubling append _doubles_ each time
2. The number of _O(1)_ appends we get until the next doubling append _also doubles_

1) and 2) somewhat cancel each other out, and we can say each append has an average/amortised cost of _O(1)_. It's not uncommon to say that dynamic arrays have a _O(1)_ time cost for appends, even though strictly speaking this is only true for the amortised cost.