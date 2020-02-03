# Hash Table
Also: _hash, hash map, map, unordered map, dictionary_

A _hash table_ organises data for quick lookup of values using a given key. 

### Strenths:
**Fast lookups:** _O(1)_ on average
**Flexible keys:** most data types can be used for keys (as long as they're hashable)

### Weaknesses:
**Slow worst-case lookups:** _O(n)_ worst-case
**Unordered:** keys aren't stored in any special order, and it could be necessary to look through every single key to find any given one
**Single-directional lookups:** going from key to value is _O(1)_ time, but looking up the value for a given key is _O(n)_ as it requires looping through the whole data set
**Not cache-friendly:** Many implementations use linked lists, which do not put data sequentially in memory

In C#, `Dictionary` is a hash table implementation.





### Hashing and Hash Functions
A _hash method_ takes in data and outputs a _hash_, which is a fixed-size string or number.

A hash can be considered a sort of fingerprint. A given file will always have the same hash, but it's not possible to go from a hash back to the original file. It's possible for several files to have the same hash value, resulting in a _hash collision_.

Hashing is useful for dictionary implementations, as well as preventing man-in-the-middle attacks.

## Built on arrays
A hash map can be considered an array add-on that allows the use of flexible keys using sophisticated hashing functions (rather than being stuck with sequential `int` indices).

If all our keys caused hash collisions, we'd be at risk of having to walk through all of our values for a single lookup (in the example above, we'd have one big linked list). This is unlikely, but it could happen. That's the worst case.

Similarly, if it were to become necessary to expand the underlying array to avoid hash collisions as things get increasingly crowded, this would require the allocation of a larger array and the rehashing of all existing keys to figure out their new positions--_O(n)_ time.

## Set
A _set_ is like a hash map, except it only stores keys.

Sets are often used to track groups of items--nodes visited in a graph, characters seen in a string, and so on. A point of interest is often whether something is in a set or not.

Sets are usually implemented in a fashion similar to hash maps, using hashing to index into an array, but without having to worry about storing values alongside keys.

In C#, `HashSet` is an implementation of sets.


