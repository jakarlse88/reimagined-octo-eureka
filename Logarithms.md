# Logarithms
The logarithm of a given number _x_ is the exponent to which another fixed number, the base _b_, must be raised, to produce that number _x_, ie. log<sub><i>b</i></sub>(_x_). Simplified: how many times must a number be multiplied by itself to get another number (ie. the power the number must be raised to)?

Logarithms are used for solving for _x_ when _x_ is in an exponent. To solve 
10<sup><i>x</i></sup> = 100:

> 10<sup><i>x</i></sup> = 100
> 
> log<sub><i>10</i></sub> (10<sup><i>x</i></sup>) = log<sub><i>10</i></sub> (100)
>
> _x_ = log<sub><i>10</i></sub> (100)
>
> _x_ = 2

## Rules
**Simplification:** log<sub><i>b</i></sub> (_b_<sup><i>x</i></sup>) = _x_

**Multiplication:** log<sub><i>b</i></sub> (_x_ * _y_) = log<sub><i>b</i></sub> (_x_) + log<sub><i>b</i></sub> (_y_)

**DIvision**: log<sub><i>b</i></sub> (_x_ / _y_) = log<sub><i>b</i></sub> (_x_) - log<sub><i>b</i></sub> (_y_)

**Powers:** log<sub><i>b</i></sub> (_x_<sup><i>y</i></sup>) = _y_ * log<sub><i>b</i></sub> (_x_)

**Change of base:** log<sub><i>b</i></sub> (_x_) = log<sub><i>c</i></sub> (_x_) / log<sub><i>c</i></sub> (_b_)



## Logarithms in algorithms
How many times must we double 1 before we get _n_, or how many times must we halve _n_ to get down to 1? The answer to both is log<sub><i>2</i></sub> (_n_).

### Binary search
Finds a target element in a sorted array.
1. Look at the middle element; is it smaller or larger than the target? Knowing this, half the array can be discarded as irrelevant.
2. Repeat 1) for the relevant side of the array. Repeat until the target is found or ruled out.

How many times must we halve our array until we've only one element left?

_n_ * (1/2) * (1/2) * (1/2) * ... = 1

or: 

_n_ * (1/2)<sup><i>x</i></sup> = 1

Which solves as:

_n_ * 1<sup><i>x</i></sup>/2<sup><i>x</i></sup> = 1

_n_ * 1/2<sup><i>x</i></sup> = 1

_n_/2<sup><i>x</i></sup> = 1

_n_ = 2<sup><i>x</i></sup>

log<sub><i>2</i></sub> _n_ = log<sub><i>2</i></sub> 2<sup><i>x</i></sup>

log<sub><i>2</i></sub> _n_ = x

Thus binary search is _O_(log<sub><i>2</i></sub> _n_).

### Sorting
_O_(_n_ log<sub><i>2</i></sub> _n_) is the best worst-case runtime we can get for comparison-based sorting. If it's possible to tightly bound the range of possible numbers, a hash map can do it in _O(n)_ with counting sort.

For example, merge sort:
1. Divides an array in half
2. Sort each half
3. Merge halves into one sorted half

How does merge sort accomplish 2)? By doing the same thing. 

The log<sub><i>2</i></sub> _n_ comes from the number of times _n_ must be halved to get subarrays of a single element, and the additional _n_ from the merging together of all _n_ items each time two sorted subarrays are merged.

### Binary trees
In a binary tree, each node has two or fewer children. A binary tree whose tiers are all full is _perfect_. If there are _n_ nodes, how high _h_ is the tree? The last level of a tree has about half the total nodes of the tree ((_n_ + 1) / 2), thus _h_ = the number of times 1 must double to get to (_n_ + 1) / 2. Thus:

h ≈ log<sub><i>2</i></sub> ((_n_ + 1) / 2)

Except...for a perfect, two-tier tree there'd be (duh) two tiers, but the number of times 1 must double to get to 2 is just 1. The height is actually one more than the number of doublings. Thus:

h ≈ log<sub><i>2</i></sub> ((_n_ + 1) / 2) + 1

h ≈ log<sub><i>2</i></sub> (_n_ + 1) - log<sub><i>2</i></sub> (2) + 1

h ≈ log<sub><i>2</i></sub> (_n_ + 1) - 1 + 1

h ≈ logc (_n_ + 1) 

## Conventions
In CompSci it's usually implied that the base is 2, thus log (_n_) usually means log<sub><i>2</i></sub> (_n_). The notation lg is also somtimes used for log base 2.