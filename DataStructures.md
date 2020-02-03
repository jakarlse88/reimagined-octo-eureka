# Data Structures

## Random Access Memory (RAM)
Conceptually, we can consider RAM as a giant bookshelf with a practically infinite number of shelves, each with its own address.

<table>
    <tr>
        <td>0</td>
        <td>00000000</td>
    </tr>
    <tr>
        <td>1</td>
        <td>00000001</td>
    </tr>
    <tr>
        <td>2</td>
        <td>00000010</td>
    </tr>
    <tr>
        <td>3</td>
        <td>00000100</td>
    </tr>
    <tr>
        <td>4</td>
        <td>00001000</td>
    </tr>
    <tr>
        <td>5</td>
        <td>00010000</td>
    </tr>
    <tr>
        <td>6</td>
        <td>00100000</td>
    </tr>
    <tr>
        <td>7</td>
        <td>01000000</td>
    </tr>
    <tr>
        <td>8</td>
        <td>10000000</td>
    </tr>
    <tr>
        <td><i>...</i></td>
    </tr>
</table>

Each shelf holds 8 _bits_ (i.e. a 0 or 1). 8 bits make up 1 _byte_.

The _processor_ (which does all the work) is connected to a _memory controller_, which performs the actual reading and writing to and from RAM and which has a direct connection to each RAM address. This direct connection is what makes the random access possible.

Processors have a series of caches (which are much faster to access than memory) in which it stores a copy of data that it's recently read from RAM. When the processor makes a request for the data at a certain memory address, the memory controller also sends the data located in a handful of nearby memory addresses, all of which the processor then caches.

The upshot of which is that reading from sequential memory addresses is faster than jumping around.

## Binary numbers
_Binary numbers_ are also called _base 2_ numbers, because each digit (or bit) can only be either 1 or 0. 

In _base 10_, the system we use daily, the _places_ are sequential powers of 10:
- 10<sup>0</sup> = 1
- 10<sup>1</sup> = 10
- 10<sup>2</sup> = 100
- 10<sup>3</sup> = 1000
- 10<sup>4</sup> = 10000
- etc.

The places in binary are sequential powers of 2:
- 2<sup>0</sup> = 1
- 2<sup>1</sup> = 2
- 2<sup>2</sup> = 4
- 2<sup>3</sup> = 8
- 2<sup>4</sup> = 16
- etc.

Thus, in base 2:
<table>
    <thead>
        <tr>
            <th>4s</th>
            <th>2s</th>
            <th>1s</th>
        </tr>
    </thead>
    <tr>
        <td>1</td>
        <td>0</td>
        <td>1</td>
    </tr>
    <tr>
        <td>= 4</td>
        <td>+ 0</td>
        <td>+ 1</td>
    </tr>
</table>

Which totals to 5.

## Fixed-width integers
1 byte can express 2<sup>8</sup> = 256 different numbers. If we had the number 256 (1111 1111) and wanted to add 1, we'd get an integer overflow which could mean either an error, or the computer discarding the required 9th bit (leaving us instead with 0000 0000).

 This is rather limiting, so we usually use either 4 (32 bits) or 8 (64 bits) bytes.
- 32-bit ints have 2<sup>32</sup> possible values (more than 4 billion)
- 64-bit ints have 2<sup>64</sup> possible values (more than 10 billion billion)

Most ints are fixed-width/fixed-length, ie. they always take up the same number of bits (that is, a 64-bit int will always take up 64 bits/8 bytes in RAM). Variable-sized numbers exist, but are rarely used. 

Fixed-width ints take up _O(1)_ (constant) space, and because of this most simple operations on fixed-width ints take _O(1)_ time as well.

## Arrays
A variable is a slot of memory (not necessarily consisting of a single byte) for storing a single value of something of any given datatype, and an array is a contiguous series of variables that can be addressed individually by means of a numeric index. 

Said indices do not necessarily correspond to actual memory addresses. For instance, for an array of 64-bit ints, each array index corresponds to 8 address slots (= 8 byte).

Grabbing the _n_<sup>th</sup> item of an array is simple:
 
> array starting address + (_n_ * size of each item in bytes).

Simple operations on fixed-width integers take `O(1)` time, and so too does this compound operation. Because the memory controller has a direct connection to each slot in RAM, we can access any given memory address in constant time as well.

tl; dr: Arrays have a `O(1)` lookup time, at the expense of each item having to be the same size and the array being contiguous in memory (ie. a potentially big block of uninterrupted memory is required).

## Strings
A string is a series of characters. Numbers are mapped to characters using a _character encoding_. One such character encoding is _ASCII_ (American Standard Code for Information Interchange).

Essentially, since characters can be expressed as 8-bit ints, strings can be expressed as arrays of 8-bit characters/ints.

| Binary   | Decimal | ASCII
| -------- | ------- | ---- |
| 01001110 |   78    |   N  |
| 01001001 |   73    |   I  |
| 01000011 |   67    |   C  |
| 01000101 |   69    |   E  |

## Pointers
An array of strings seems initially problematic, in that it seemingly violates the rule of each element needing to be of the same size (as strings aren't often the same length unless required to be so by some arbitrary constraint).

What actually happens is that the strings themselves are stored wherever there's place for them in memory, and the elements in our array actually hold only the _addresses_ (as `*int`) of their corresponding strings.

> Pointers are marked with a *

The tradeoff of this flexibility is that the pointers in this array make it **not cache-friendly**, because the memory controller only sends the contents of nearby memory addresses to the processor for caching with each read.

Note that this slowdown is _not_ reflected in Big O time cost; lookup is still nominally _O(1)_ time.

## Dynamic arrays
The classical static array requires that we specify upfront how many indices we want it to have upon declaration. It is however possible to program an array to resize itself when it runs out of space; this is known as a dynamic array. 

Upon allocation of a dynamic array, an underlying static array is made (the size of which is implementation-specific). An `end_index` variable keeps track of where the dynamic array ends and any extra capacity begins. As data is appended, at some point the underlying static array will become full and upon the next addition the dynamic array implementation will need to:

1. Make a new and bigger array  
2. Copy each element from the old array into the new array
3. Free up the old array
4. Append the new element(s)

Most often the new array is double the size of the old array. Appending an item to an array is an _O(1)_ operation, however a single doubling append an _O(n)_ operation (as all _n_ elements must be copied over).

Importantly, while the time cost of each _O(n)_ doubling append doubles each time, the _number of O(1) appends_ we get until the next doubling append also doubles. Thus, each append has an average or amortised cost of _O(1)_--the worst case is still _O(n)_.

Dynamic arrays have the advantage of not needing their sizes specified in advance, and the drawback that some appends can be expensive.

## Linked lists
A linked list is a data structure consisting of a collection of nodes representing a sequence, and containing both data and a reference to the next node in the sequence (`*next`, for simplicity).

These nodes can be stored wherever in memory there's free space, and don't need to be in order. The first node is called the _head_, and the last node is called the _tail_ (mostly--sometimes the tail may also refer to everything after the head). 

A `*head` is essential, lest it become unable to find the start of the list. A `*tail` can also be useful, eg. for easy appends. To append an item to a linked list:

1. Create a new node `d` 
2. Find the (previous) last node `c` 
3. Point `c`'s `*next` to `d` 
4. Point `*tail` to `d`

No matter how many elements in the list, the operation is the same, thus the worst-case append is _O(1)_ (which is better than the worst-case for dynamic arrays; however, the average case for both is constant time).

Prepending to a linked list is an equally simple and also _O(1)_ operation, which beats the dynamic array prepend operation's _O(n)_. 

Lookups with linked lists are more involved because we can't know where in memory the _i_<sup>th</sup> is located and thus have to traverse the list node for node until we find it.  

```C
public static LinkedListNode GetIthItemInLinkedList(LinkedListNode head, int i)
{
    if (i < 0)
    {
        throw new ArgumentException($"i can't be negative: {i}", nameof(i));
    }

    var currentNode = head;
    var currentPosition = 0;

    while (currentNode != null)
    {
        if (currentPosition == i)
        {
            // Found it!
            return currentNode;
        }

        // Move on to the next node
        currentNode = currentNode.Next;
        currentPosition++;
    }

    throw new ArgumentException($"List has fewer than i + 1 ({i + 1}) nodes", nameof(head));
}
```

Linked lists have _O(n)_-time lookups, and said operation is not cache-friendly. 

## Hash tables
A hash table is a data structure that can map keys to values, using a _hashing function_ to compute an index into an array of slots from which the desired value can be found.

For example, in the case of keeping track of how many times a word appears in a text we can use the word itself as the key (summing its characters' values), compute an index from that key using a hashing function, and store the count (value) in that index of the array.

It's not unlikely that two or more words will add up to the same value, and thus produce the same index when hashed. In this case, we have a _hash collision_. One common way of dealing with hash collisions is having each array slot holding a pointer to a linked list holding the counts for all the words that hash to that index (with each node storing the word itself, as well as the count and a `*next` pointer).

In the worst case, lookup for hash tables degrade from _O(1)_ to _O(n)_. However, it is commonly accepted that collisions are rare enough (especially with thorough modern implementations) that on average, lookup is _O(1)_.

tl; dr Hash tables provide fast lookups by key, but some lookups could be slow.



