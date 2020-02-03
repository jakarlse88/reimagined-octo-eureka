# Call Stack
The _call stack_ (a _LIFO_ data structure) is what's used by a program to keep track of its function/method calls. The call stack is made up of _stack frames_—one for each function/method call. 

Quoth [Wikipedia](https://en.wikipedia.org/wiki/Call_stack):

> Since the call stack is organized as a stack, the caller pushes the return address onto the stack, and the called subroutine, when it finishes, pulls or pops the return address off the call stack and transfers control to that address. If a called subroutine calls on yet another subroutine, it will push another return address onto the call stack, and so on, with the information stacking up and unstacking as the program dictates. If the pushing consumes all of the space allocated for the call stack, an error called a stack overflow occurs, generally causing the program to crash. Adding a subroutine's entry to the call stack is sometimes called "winding"; conversely, removing entries is "unwinding".

## Stack Frame
A stack frame usually stores:
- local variables
- arguments passed into the method/function
- information about the caller's stack frame
- a _return address_, ie. what the program should do after the method/function returns

(Note that some specifics vary. AMD64 (64-bit x86) processors pass some arguments in registers and some on the call stack, and ARM processors store the return address in a special register instead of on the call stack.)

### Stack frames and space cost
Each method/function creates its own stack frame, taking up space on the call stack. This can impact the space complexity of an algorithm-particularly when recursion is used.

For instance, to multiply all numbers between 1-_n_ this recursive approach could be used:

```C#
public int Product1ToN(int n)
{
    // Assuming n >= 1
    return (n > 1) ?
        (n * Product1ToN(n - 1)) :
        1;
}
```

_n_ = 10 would result in the following call stack:
- `Product1ToN()` _n_ = 1
- `Product1ToN()` _n_ = 2
- `Product1ToN()` _n_ = 3
- `Product1ToN()` _n_ = 4
- `Product1ToN()` _n_ = 5
- `Product1ToN()` _n_ = 6
- `Product1ToN()` _n_ = 7
- `Product1ToN()` _n_ = 8
- `Product1ToN()` _n_ = 9
- `Product1ToN()` _n_ = 10
 
 The entire stack takes up _O(n)_ space, even though the method doesn't create any data structures.

 An iterative implementation could look like this:

 ```C#
 public int Product1ToN(int n)
 {
     // Assuming n >= 1
     int result = 1;

     for (int i = 1; i <= n; i++)
     {
         result *= i;
     }

     return result;
 }
 ```

 This implementation takes up a constant amount of space. At the beginning of the loop, the call stack looks as follows:
 - `Product1ToN()` `n` = 110, `result`= 1, `i` = 1
 
 As the method executes the local variables will change, but since no other methods are called execution remains within the same stack frame. 

 **In general, although a compiler or interpreter will take care of the management of the call stack, we have to consider the depth of the call stack when analyzing the space complexity of a given algorithm.**

Recursive functions especially can end up building huge call stacks, potentially even resulting in a _stack overflow_ (at which point there's simply no more space to be had).

If the very last thing a method does is to call another method its stack frame might not be needed anymore, and it could free up its stack frame before doing its final call in order to save space; this is known as _tail call optimisation_ (TCO). If a recursive method/function is TCO-optimised it may not end up with this huge call stack. However, in general, most languages don't provide TCO. Scheme guarantees TCO; some Ruby, C, and JavaScript implementations _may_; Python and Java decidedly _don't._