listee
===

C++ list comprehensions! Well, almost.....

This project attempts to bring python's _list comprehensions_ to C++. You see,
in python, you can do magic like this:

    l1 = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    l2 = [x for x in l1 if x%2 == 0]
    # l2 = [2, 4, 6, 8, 10]

C++ doesn't let you do this, but with the advent of lambdas and other
functional magic, I feel like it really should let you. As such, I'm going to
attempt to bring these to C++.

In other words, the C++ equivalent would be:

    auto l1 = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    listee::expression x;
    auto l2 = x with x in l1 when x % 2 == 0;

Isn't this cool!

Eventually, I expect all of the following to work:
    
    auto l1 = {1, 3, 5, 7, 9};
    auto l2 = {2, 4, 6, 8, 10};
    listee::expression x, y;
    auto l3 = x*y with x in l1 with y in l2 when x + 3*y <= 100;

    auto l4 = x*y with x in l1 with y in l2 when x < 4 and y < 10;

    auto l5 = x / y with x in l3 with y in l4 when x < 3 and x % y > 2;


## Features ##
The obvious feature is the wonderful new syntax you're able to use, simply by
adding ```#include <listee>``` to your code. However, on top of this:
* Asynchronous/lazy evaluation. In the above example, l2 will be evaluated
  either a) in the background while your program does nothing, or waits for
  input, or etc, or b) only when you need its contents. This lazy/asynchronous
  behaviour is provided by the magical new standard interface of C++11, and
  requires no external dependencies
* No external dependencies. No boost, no library that you have to compile from
  source but never compiles, nothing. The only required header files and
  libraries are those in the C++11 standard.
* Fun. Seriously, you've always *wanted* to use this python-esque syntax. You
  know it'll reduce bugs, because you don't have to write 90 lines of code
  every time you want to do it. You don't have to worry about writing templated
  functions and types and all sorts of voodoo witchcraft just to make your code
  compile. And now you can.

### Limitations ###
Obviously, as far as emulating python syntax goes, our hands are tied; we
simply can't use 'for' and 'if' like python does, as these are key language
constructs which we're not allowed to abuse. This forces us to use 'with',
'in' and 'when' rather than 'for', 'in' and 'if' (though hey, I like it; it's
the C++ touch!).

Further, unfortunately, the variables you use in your expression have to be
predeclared. I've puzzled over this for a few hours (and enlisted the help of
many on StackOverflow) but have come up with no solution; feel free to contact
me if you have an idea.

Finally, it also means your program can't use the variables / names 'with',
'in' and 'when'. These are declared as ```#define``` macros, so using them in
your program will mean Bad Things happen.
