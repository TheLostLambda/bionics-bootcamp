---
title: "Modern Python Scripts"
date: 2020-10-13T18:04:28+01:00
authors: ["tll"]
categories:
  - Software
tags:
  - Week 2
  - Level 2
  - Intermediate
  - Python
  - Worksheet
  - Programming
---

# Important Note

This is worksheet is meant to be interactive! I'd **highly recommend** you copy [this document](https://colab.research.google.com/drive/109bNJVHVMy3DDbrqyf6kjB1u6HBnSW7E?usp=sharing) to your Google Drive and go through it in Google Colaboratory. This page is here primarily for archival purposes.

# Exercises!

This week's exercises are available as a separate worksheet [here](https://colab.research.google.com/drive/1IwiAI7D-6QdXpC1yR6JNSC8yQ560dcj0?usp=sharing). Don't hesitate to ask any questions you might have in the Discord!

# Modern Python Scripts

While you've already learned enough Python to write some decently complex programs, you may find lots of your code to be repetitive and, as it grows in size, increasingly difficult to read.

This worksheet will explain some of the most important tools for writing not just functional, but maintainable and understandable code. We'll also touch on some more advanced features of the Python language that you'll need to write more sophisticated programs.

## Important Resources
### Getting help and feedback

Even if you are going through this worksheet outside of a scheduled session time, you can always find help on the [Sheffield Bionics Discord](https://discord.gg/N4k7ECt)! Just drop a message in the #programming channel or directly message one of our coding instructors.

### Tools for writing Python code

For this course, we'll be working primarily in [Google Colaboratory](https://colab.research.google.com) and [repl.it](https://repl.it/) so that you don't need to install anything on your computer. If, however, you are interested in installing Python locally on your computer, you can find [detailed instructions here](https://realpython.com/installing-python/). You may also be interested in something like [Spyder](https://www.spyder-ide.org/) or [PyCharm](https://www.jetbrains.com/pycharm/) which make writing Python scripts a bit nicer.

# Getting Funky with Functions
 
Seeing as the point of writing computer programs was to automate repetitive tasks, writing repetitive code feels a bit like trading one problem for another; luckily, we can do better. Instead of leveraging copy-paste, we can bundle up reusable chunks of code into _functions_ – this allows us to reuse code without duplicating it. That sounds great in theory, but what actually is a function?

Conceptually, you can think of functions as a mini-programs that take their own set of inputs and perform some calculation before returning control to the parent program. That's enough word-soup though, let's take a look at a (very simple) real world example.


```python
def double(x):
    """Doubles some number, returning the result"""
    return x + x

print('Double 4 is:', double(4))
print('Double the double of 4 is:', double(double(4)))
help(double)
```

    Double 4 is: 8
    Double the double of 4 is: 16
    Help on function double in module __main__:
    
    double(x)
        Doubles some number, returning the result
    


This example introduces a number of new things! Don't panic, we'll work through this together. Let's start from the top:

`def double(x):`

This first line is what actually defines our new function. All function definitions start with the `def` keyword and are followed by the name of the function – in this case, that's `double`. When defining your own functions, you should strive to pick descriptive names so others reading your code (or yourself, a few months down the line) can infer what they do.

The brackets – `()` – contain the inputs or "arguments" of the function. In this case, there is a single argument named `x`. Whenever we'd like to refer to this functions input, we can use the variable `x`; we do so in the final line of the function: `x + x`.

Finally, as with `while` and `if`, the colon (`:`) introduces a block of grouped code. In this case, all of the indented lines following the `:` belong to the function `double`.

`"""Doubles some number, returning the result"""`

This line looks somewhat familiar, but has something odd going on: what is up with all of those `"`'s? This is a special type of string in Python called a multi-line string. This means that, whatever is between all of those quotes, acts just like a normal string, but can stretch over several lines. That allows us to do things like the following:

```python
"""Doubles some number,
returning the result"""
```

Try changing the code for `double` so that the string stretches over several lines (like the example above). Does this work with a regular string (just a single `"` or `'`)?

That's fine and good, but what is this random string doing at the beginning of our function? Well, in Python, a string appearing right after the definition of a function is treated as a "documentation string" or "docstring" for short. This docstring is meant to explain the purpose of the function and indicate to a programmer how they might make use of it. These are **optional**, but strongly encouraged for larger functions. If you'd like to see  information about the `double` function (or any other function in Python), you can use `help()`. If you'd like to learn a bit more about `print()`, for example, try running `help(print)`.

`return x + x`

Finally, we have the new `return` keyword. In this case, `x + x` happens first, giving a doubled version of `x`, then the `double` function takes this value and _returns_ it to the part of the program that called it. This is the "output" of the function. We see where this value ends up in the next line:

`print('Double 4 is:', double(4))`

To demonstrate how functions are called in Python, let's play computer for a moment and evaluate this code one step at a time. We start by looking at the arguments of the `print()` function:

`'Double 4 is:'` and `double(4)`

Well, as we've seen before, the `'Double 4 is:'` is just a string, so we can just leave that part of the code as-is for the moment – there is nothing more we can do with it right now.

Moving on to `double(4)`, this actually looks like some Python code that we can run! The first thing we do is look for a function named `double()`. Lo and behold, we've got one! Just as a reminder, the function looks like this:

```python
def double(x):
    """Doubles some number, returning the result"""
    return x + x
```

After finding this function, Python pauses whatever it was in the middle of and jumps instead to the `double()` function. Looking at the definition of the `double()` function, we can see that it wants exactly one argument. Looking back at our function-call from earlier (`double(4)`), we can see that this argument is `4`. Before running the rest of the function, we implicitly set the variable `x` equal to `4`:

```python
x = 4
```

With that out of the way, we can move on to actually running the function. In this case, the docstring is ignored, so we jump straight to the `return x + x` line.

Following our inside-out evaluation, we start by computing `x + x`. In this case, Python looks up the value of the variable `x` and finds that it's equal to `4` (following our earlier implicit assignment). Substituting for the value of `x`, our code now becomes `4 + 4`.

Now things start falling into place; `4 + 4` just becomes `8` and can't be evaluated any further. Zooming back out, we now have `return 8` which tells Python we are finally done with the `double()` function and to return `8` to the caller. Zooming out even further, our `double(4)` is substituted for it's return value of `8`, leaving us with:

```python
print('Double 4 is:', 8)
```

At last, it's somewhat obvious what the program will do. Running this line of code prints:

`Double 4 is: 8`

The second `print()` line is quite similar, but jumps back into the `double()` function with `x = 8`. The whole evaluation of this line is left as an exercise to the reader (as I'm rather sick of typing <3), but don't hesitate to ask on the Discord if you still have questions!

## Getting in(to) Arguments

While that's just about everything you need to know about using and evaluating functions, there is a bit more to defining and supplying function arguments – let's take a look:


```python
# An example of several arguments, comma separated
def triangle(base, height):
    print('The triangle is', base, 'units wide and', height, 'units high')

# Called positionally
triangle(8,4)
triangle(4,8)

print() # Print a blank line

# Called as keywords
triangle(base=8, height=4)
triangle(height=4, base=8)

# An example of an optional argument with a default
def say(sentence, shout = False):
    if shout:
        sentence = sentence.upper()
    print(sentence)

say('Hello')
say('Hello', True)
say('Hello', shout = False)
```

    The triangle is 8 units wide and 4 units high
    The triangle is 4 units wide and 8 units high
    
    The triangle is 8 units wide and 4 units high
    The triangle is 8 units wide and 4 units high
    Hello
    HELLO
    Hello


That's rather subtle, but the take-away is that the arguments of a function can (usually) be specified in one of two ways: positionally or as a keyword. When arguments are specified positionally, as in `triangle(8,4)`, then the first number is bound to `base` and the second to `height` as you'd expect. Predictably, flipping this order, as in `triangle(4,8)`, flips the bindings: `base` being `4` and `height` being `8`.

What's new here, is how we specify keyword arguments. Keyword arguments are given in the form `name=val`, where `name` is the name of the argument in the function definition and `val` is some value. As the second example above demonstrates, you can give these keyword arguments in any order you'd like without changing the behavior of the function.

Finally, in the `say()` function, we see how optional arguments can be defined. They look a lot like passing keyword arguments, but the `name=val` is in the function definition. If you call `say()`, but leave out the second argument (as in the first invocation), then `shout` is set to the default `False` value given in the `def` line. If you do specify the second argument, in either the positional (second invocation) or keyword (third invocation) form, then the default value of `shout` is overridden.

## Being More Flexible

The last thing we'll take a look at is how functions can take an unknown or variable number of arguments! You may have noticed that you can give `print()` as many or as few arguments as you'd like (`print()` is valid and so is `print(1,2,3,4,5)`), but how does this work? Let's take a look:


```python
def count_args(*numbers):
    print('You passed', len(numbers), 'arguments:', numbers)

count_args(1,2,3,4,5)
count_args('Howdy', True, 3.14)
count_args()
```

    You passed 5 arguments: (1, 2, 3, 4, 5)
    You passed 3 arguments: ('Howdy', True, 3.14)
    You passed 0 arguments: ()


By adding the star (`*`) in front of the `numbers` argument, we've indicated to Python that we'd like to take a variable number of arguments and store them in the list named `numbers`. We can then get the length of the list and print it out.

But hold on... `numbers` was printed with round, `()`, not square `[]` brackets... What gives? Well, strictly speaking, `numbers` isn't a list; it's a tuple. What's the difference? Well, as far as we are concerned, very little. All we need to know is that tuples are immutable, so you can't change them once they've been defined. In an example:


```python
# This is chill
lst = [1,2,3,4,5]
print(lst)
lst[2] = '|'
print(lst)
lst.append(6)
print(lst)
```

    [1, 2, 3, 4, 5]
    [1, 2, '|', 4, 5]
    [1, 2, '|', 4, 5, 6]



```python
# Not so chill...
lst = (1,2,3,4,5)
print(lst)
lst[2] = '|'
print(lst)
lst.append(6)
print(lst)
```

    (1, 2, 3, 4, 5)



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-218-d2ac872bb0ff> in <module>
          2 lst = (1,2,3,4,5)
          3 print(lst)
    ----> 4 lst[2] = '|'
          5 print(lst)
          6 lst.append(6)


    TypeError: 'tuple' object does not support item assignment


The first example uses lists that can be mutated (changed), but the tuple throws an error.

## The Key(word)s to Success

Right... So _maybe_ these section titles are a bit contrived... Regardless, there is one final trick to function arguments in Python: taking a variable number of **keyword** arguments. Let's look at a quite similar example to the last one:



```python
def count_kwargs(**numbers):
    print('You passed', len(numbers), 'keyword arguments:', numbers)

count_kwargs(a=1,b=2,c=3,d=4,e=5)
count_kwargs(cry='Howdy', lie=True, pi=3.14)
count_kwargs()
```

    You passed 5 keyword arguments: {'a': 1, 'b': 2, 'c': 3, 'd': 4, 'e': 5}
    You passed 3 keyword arguments: {'cry': 'Howdy', 'lie': True, 'pi': 3.14}
    You passed 0 keyword arguments: {}


Well the number of arguments is still what we'd expect, which is good, but what in the world is going on with that curly bracket (`{}`) mess? Well...

# Is that a Dict in your pocket? Or are you just happy to see me?

Sorry about that one... But, the curly braces introduce us to a new type of data: the dictionary! Dictionaries act a lot like the lists we've already seen, but instead of each value having a numerical index, they have keys. In the dictionary `{'cry': 'Howdy', 'lie': True, 'pi': 3.14}`, `'cry'`, `'lie'`, and `'pi'` are the keys and `'Howdy'`, `True`, and `3.14` are the values. Just as you can access list-items by their index, you can access dictionary values by their key:


```python
# Dictionary from above
truths = {'cry': 'Howdy', 'lie': True, 'pi': 3.14}
print(truths['pi'])
print(truths['cry'])
print(truths['lie'])

print() # Blank line

# The keys don't have to be strings
lies = {True: False, 42: 'the answer', (1,2,3): [7,7,7]}
print(lies[True])
print(lies[42])
print(lies[(1,2,3)])
```

    3.14
    Howdy
    True
    
    False
    the answer
    [7, 7, 7]


Dictionaries are helpful for when you want to associate two types of data – like a person with their favorite numbers. We can use that example to explore some the methods provided by dictionaries:


```python
# Our dictionary
favorite_numbers = {
    'Brooks': [42, 2.72, 6.28],
    'Artemis': [93, 4, -1],
    'Wolfgang': [0, 3, 7],
}

# All of the keys
print(favorite_numbers.keys())

# All of the values
print(favorite_numbers.values())

# Why not both?
print(favorite_numbers.items())
```

    dict_keys(['Brooks', 'Artemis', 'Wolfgang'])
    dict_values([[42, 2.72, 6.28], [93, 4, -1], [0, 3, 7]])
    dict_items([('Brooks', [42, 2.72, 6.28]), ('Artemis', [93, 4, -1]), ('Wolfgang', [0, 3, 7])])


Well, `.keys()`, and `.values()` return about what you'd expect: all of the keys and values of the dictionary respectively. The `.items()` method seems to return a list of both, paired up in tuples. The odd `dict_keys`, `dict_values`, and `dict_items` are worth a mention as well. Strictly speaking, these aren't normal lists. These are "objects" (which you'll learn much more about later) that update automatically when the dictionary does. While we can convert these into normal lists, (using the `list()` function), they can also be used in a way we've not seen before – in a `for` loop.


```python
# Our dictionary
favorite_numbers = {
    'Brooks': [42, 2.72, 6.28],
    'Artemis': [93, 4, -1],
    'Wolfgang': [0, 3, 7],
}

for name in favorite_numbers.keys():
    print('A name:', name)

print()
    
for numbers in favorite_numbers.values():
    print('Some numbers:', numbers)

print()

for item in favorite_numbers.items():
    print('An item:', item)
```

    A name: Brooks
    A name: Artemis
    A name: Wolfgang
    
    Some numbers: [42, 2.72, 6.28]
    Some numbers: [93, 4, -1]
    Some numbers: [0, 3, 7]
    
    An item: ('Brooks', [42, 2.72, 6.28])
    An item: ('Artemis', [93, 4, -1])
    An item: ('Wolfgang', [0, 3, 7])


Well that's a lot to work our way through, let's start with the `for` loop:

# Looping back for a minute

`for name in favorite_numbers.keys():`

A `for` loop is like a `while` loop in that it repeats some block of code a number of times, but while `while` loops loop as long as some condition is true, `for` loops loop through collections of values. These collections can be lists, strings, tuples, dictionaries, and more. The differences between a `for` and `while` loop might be easiest to see side-by-side:


```python
# Print all the values of a list
i = 0
lst = ['Howdy', 42, ('over', 9000)]
while i < len(lst):
    print(lst[i])
    i += 1
```

    Howdy
    42
    ('over', 9000)



```python
# The same, but nicer
for item in ['Howdy', 42, ('over', 9000)]:
    print(item)
```

    Howdy
    42
    ('over', 9000)


In essence, `for` loops run once for every item a collection (given after the `in`) and let you access the current item via the variable given _before_ the `in`. By combining them with another new construct, the `range()` function, we can almost entirely replace our crusty `while` loops:


```python
for x in range(1,4):
    print(x, 'mississippi...')
```

    1 mississippi...
    2 mississippi...
    3 mississippi...


Ranges are, like list slices, inclusive on the lower bound, and exclusive on the upper bound. That's why `range(1,4)` actually generates the values 1, 2, and 3. You can even give `range()` a "step":


```python
# Converting to a list so we can see the result:
list(range(0, 11, 2))
```




    [0, 2, 4, 6, 8, 10]



While `for` loops are generally much cleaner than `while` loops, we might occasionally still want access to the list indices, as in the following example:


```python
# Print all the values of a list with their index
i = 0
lst = ['Howdy', 42, ('over', 9000)]
while i < len(lst):
    print(i, ':', lst[i])
    i += 1
```

    0 : Howdy
    1 : 42
    2 : ('over', 9000)


Luckily, we can have the best of both worlds using a function called `enumerate()`:


```python
# Print all the values of a list with their index
for item in enumerate(['Howdy', 42, ('over', 9000)]):
    print(item)
```

    (0, 'Howdy')
    (1, 42)
    (2, ('over', 9000))


Err... That's not quite right... It's close though, let's just pull out the individual parts of the tuple:


```python
# Print all the values of a list with their index
for item in enumerate(['Howdy', 42, ('over', 9000)]):
    print(item[0], ':', item[1])
```

    0 : Howdy
    1 : 42
    2 : ('over', 9000)


There we go! That's looking a bit better! But hey, since I seem to hate indexing, can we get rid of that too? Well, let's **unpack** that problem together:


```python
# Unpacking a tuple into individual variables
first, second, third = ('Howdy', True, 42)
print(first)
print(second)
print(third)
```

    Howdy
    True
    42


By placing several variables before the `=` and separating them with commas, we are able to break up or _unpack_ the values of the tuple. As it happens, `for` loops also allow unpacking of the values we are iterating over:


```python
# Print all the values of a list with their index
for i, item in enumerate(['Howdy', 42, ('over', 9000)]):
    print(i, ':', item)
```

    0 : Howdy
    1 : 42
    2 : ('over', 9000)


Finally I can sleep easy and free of superfluous code...

On a related note, you can abort any type of loop early with a `break` statement:


```python
# Bail after index 1
for i, item in enumerate(['Howdy', 42, ('over', 9000)]):
    print(i, ':', item)
    if i == 1:
        break
```

    0 : Howdy
    1 : 42


To rewind back to the start of this tangent, we can also use this unpacking when iterating over the key-value pairs of a dictionary. Let's take a look:


```python
# Our dictionary
favorite_numbers = {
    'Brooks': [42, 2.72, 6.28],
    'Artemis': [93, 4, -1],
    'Wolfgang': [0, 3, 7],
}

for key, value in favorite_numbers.items():
    print(key, ':', value)
```

    Brooks : [42, 2.72, 6.28]
    Artemis : [93, 4, -1]
    Wolfgang : [0, 3, 7]


# Another Dict joke (and some about lists too)

We are getting pretty good at working with collections in Python, but there are a number of tricks we've yet to see. Time for some speed-dating:

## List Methods


```python
# Popping is the reverse of .append()
lst = [1, 2, 3]
print(lst)
# Append a new element
lst.append(4)
print(lst)
# Pop off the last element and return it
last = lst.pop()
print(last, ';', lst)
```

    [1, 2, 3]
    [1, 2, 3, 4]
    4 ; [1, 2, 3]



```python
# Inserting a value into the middle of a list
lst = ['Hello', 'General', 'Kenobi']
print(lst)
# Use .insert(position, value)
lst.insert(1, 'There')
print(lst)
```

    ['Hello', 'General', 'Kenobi']
    ['Hello', 'There', 'General', 'Kenobi']



```python
# Finding a value in a list
lst = ['Nothing', 'to', 'see', 'secret', 'here']
print('The secret is at index:', lst.index('secret'))
```

    The secret is at index: 3



```python
# Reversing a list
lst = [1, 2, 3, 4, 5]
# This returns a reversed list, but doesn't change lst
# reversed() returns a not-quite-list, so we need to convert it with list()
print(list(reversed(lst)))
print(lst)
# .reverse() returns nothing, but changes the underlying lst
print(lst.reverse())
print(lst)
```

    [5, 4, 3, 2, 1]
    [1, 2, 3, 4, 5]
    None
    [5, 4, 3, 2, 1]



```python
# Checking if an item is in a list
fruit = ['apple', 'orange', 'strawberry']
print('Is a potato a fruit?:', 'potato' in fruit)
print('Is an apple a fruit?:', 'apple' in fruit)
```

    Is a potato a fruit?: False
    Is an apple a fruit?: True



```python
# Removing items from a list
fun_things = ['frisbee', 'desk work', 'chatting', 'games']
# If you're not a fan of cubicles
print(fun_things)
del fun_things[1]
print(fun_things)
# If you're an avid gamer
del fun_things[:2]
print(fun_things)
```

    ['frisbee', 'desk work', 'chatting', 'games']
    ['frisbee', 'chatting', 'games']
    ['games']


Wow! That was a lot! It's okay though, it gets better: much of the functionality that applies to lists, also applies to dictionaries.

## Dictionary Methods


```python
# Some of the list methods that also apply to dictionaries
truths = {'cry': 'Howdy', 'lie': True, 'pi': 3.14}

# pop() is quite similar, but needs a key
print(truths)
lie_value = truths.pop('lie')
print(lie_value, ';', truths)

print()

# in can be used to check for particular keys
print("Is a there a 'cry' key?:", 'cry' in truths)
print("Is a there a 'lie' key?:", 'lie' in truths)

print()

# The del keyword can delete entries
print(truths)
del truths['cry']
print(truths)
```

    {'cry': 'Howdy', 'lie': True, 'pi': 3.14}
    True ; {'cry': 'Howdy', 'pi': 3.14}
    
    Is a there a 'cry' key?: True
    Is a there a 'lie' key?: False
    
    {'cry': 'Howdy', 'pi': 3.14}
    {'pi': 3.14}



```python
# Combining dictionaries
pet_store = {'cats': 4, 'dogs': 2, 'fish': 8}
ps2 = {'cats': 6, 'lizards': 5}

print(pet_store)
pet_store.update(ps2)
print(pet_store)
```

    {'cats': 4, 'dogs': 2, 'fish': 8}
    {'cats': 6, 'dogs': 2, 'fish': 8, 'lizards': 5}


Note that combining dictionaries doesn't do anything fancy like adding the number of `'cats'`, it just adds new keys and overwrites existing ones.

### Dictionaries in Different Python Versions

Dictionaries have changed a couple of times in the recent versions of Python. The latest changes are so new, I can't show them off yet! The most recent changes are, as of Python 3.8, dictionaries being ordered (so you can do things like use `reversed()` on them), and, as of 3.9, there is a new "merge" operator that returns the combination of two dictionaries (it looks like `old_dict | new_dict`). You can [learn more about the latest changes here.](https://docs.python.org/3/library/stdtypes.html#dict)

# Modules for Code Reuse

Even if you are following best practices and organize your code into reusable functions, at some point your Python files will grow too big to handle. To help cut through the chaos, Python lets you split your code into  files called "modules". When these modules are bundled up and shared with others, then they are referred to as _libraries_. All Python files are modules by default, but if you want to use code from another module in your file, you'll need to make use of the `import` statement.

Python also ships with number of built-in modules that are always available. Let's try using code from one of those now!


```python
# Import the built-in random module
import random

# A regular import like above means that we need to type `random.` before the name of
# whatever function we'd like to use. This one gives a random number between 0 and 1:
print('Different every run!', random.random())

# This shuffles the order of items in a collection
lst = [1,2,3,4,5]
print('List:', lst)
random.shuffle(lst)
print('Shuffled:', lst)
```

    Different every run! 0.6238241916384651
    List: [1, 2, 3, 4, 5]
    Shuffled: [2, 5, 4, 1, 3]


Try running the code above a couple of times and watch how the results change! By default, when you import something, you need to type the name of the module _and_ the name of the function (like `random.random()` or `random.shuffle()`). However, if you are using these functions often, this starts to look like needless repetition! Luckily, Python also gives us a way to import functions without that prefix using the `from` keyword. Let's take a look:


```python
# Import two particular functions without the prefix
from random import random, shuffle

# Same as above, but without the prefixes
print('Different every run!', random())

lst = [1,2,3,4,5]
print('List:', lst)
shuffle(lst)
print('Shuffled:', lst)
```

    Different every run! 0.5655186324313067
    List: [1, 2, 3, 4, 5]
    Shuffled: [3, 4, 5, 2, 1]


This `from ... import ...` construction is quite common in Python and is something you'll see often. If you want to import **all** of the functions in a module without their prefix, you can write the following:


```python
# This imports *everything* without a prefix
# This is generally considered bad practice!
from random import *

# Same as above, but without the prefixes
print('Different every run!', random())

lst = [1,2,3,4,5]
print('List:', lst)
shuffle(lst)
print('Shuffled:', lst)
```

    Different every run! 0.43020718250620593
    List: [1, 2, 3, 4, 5]
    Shuffled: [4, 3, 5, 1, 2]


The star or glob (`*`) import is often helpful when you need all of the definitions from within a module, but is considered bad practice when you are only using a couple of functions like we are above. Glob imports can make it rather unclear what module certain functions belong to and could potentially lead to name clashes. If you'd previously defined a `seed()` or `choice()` function, for example, it would be overridden by the glob import.


```python
def seed():
    print('Planting a pretty tree!')

print('Pre-import')
seed()

from random import *
    
print('Post-import')
seed() # None of these do anything?!
seed()
seed()
```

    Pre-import
    Planting a pretty tree!
    Post-import


As you can see, while glob imports might save you some typing, they can also cause some headaches. Use your judgment to apply them sparingly!

# Something to File Away...

Our final topic will be file handling in Python. Knowing how to handle files is valuable in its own right, but this will also serve as an introduction to a few new constructs. Let's start by `open()`ing and `write()`ing to a file:


```python
# The first argument is the filename; the second, optional argument is the "mode"
file = open('story.txt', 'w')

# Write to the file
file.write('Once upon a time, there was a brave programmer.\n')

# Close the file! It's vitally important that you remember this!
file.close()

# List the files in the current directory. Can you see story.txt?
import os
os.listdir()
```




    ['__pycache__',
     'level1.py~',
     'level3.py~',
     'level3.py',
     '.ipynb_checkpoints',
     'Level 1 (Basics of Programming & Python).ipynb',
     'level1.py',
     'Planning.org',
     'level2.py~',
     'level2.py',
     'pig_test.txt',
     'translated_pig_test.txt',
     'Level 2 (Modern Python Scripts).ipynb',
     'Level 1 (Basics of Programming & Python).md',
     'Planning.md',
     'story.txt',
     'pets.txt',
     'Level 2 Exercises.ipynb']



Depending on where you are running this notebook, the block above should have either written a file to your computer or to the cloud (as in the case of Google Colaboratory). Let's break that down line-by-line.

`file = open('story.txt', 'w')`

This first line makes use of the `open()` function to either open or create the `story.txt` file. The `'w'` indicates to Python that we plan on writing to this file (and not reading!). There are a number of other modes available: `'r'` opens for reading, `'a'` for appending (adding to the end of the file), and `'+'` for writing _and_ reading. You can find out [more about modes here](https://docs.python.org/3/library/functions.html#open). Once the file has been opened, we store its _handle_ in the `file` variable. A file handle represents an open file, but is distinct from the contents of the file – if we want to actually see what's in the `file`, we'll have to call `.read()` on it.

`file.write('Once upon a time, there was a brave programmer.\n')`

The `.write()` method works a little like `print()`, but prints to a file instead of to the console. In this case, we are writing `Once upon a time, there was a brave programmer.` to the first line of the file. Unlike `print()`, `.write()` only takes one argument (a string) and doesn't automatically move to the next line – that is done by the special "newline" character: `\n`. This won't actually write `\n` to the file, but will act as if we'd manually pressed the ENTER key. We can recognize the backslash (`\`) as the same character we used to escape nested quotes, as in `'It\'s a travesty'`.

`file.close()`

This very important line tells the computer you are done working with `file` (for now). Leaving out this line could result in a number of strange errors: your edits might not appear in the file until later, you may run out of file handles, etc. Luckily, there is a better way to automatically close your files when you are done with them, something called `with`, which we will explore further in just a moment. Before that, these last lines:

`import os`

`os.listdir()`

First we import the built-in `os` library. This contains a number of tools for interacting with the operating system (usually Mac, Windows, or Linux). One of these tools is the `listdir()` function which shows us what files are in our current folder / directory. Don't worry too much about this for now, but if you are curious, you can find a lot more about the `os` library [here](https://docs.python.org/3/library/os.html).

Righto! I promised that `with` could help us clean things up a little. Let's take a look:


```python
# Open the file in a `with` block
with open('story.txt', 'w') as file:
    # Write to the file
    file.write('Once upon a time, there was a brave programmer.\n')

# List the files in the current directory. Can you see story.txt?
import os
os.listdir()
```




    ['__pycache__',
     'level1.py~',
     'level3.py~',
     'level3.py',
     '.ipynb_checkpoints',
     'Level 1 (Basics of Programming & Python).ipynb',
     'level1.py',
     'Planning.org',
     'level2.py~',
     'level2.py',
     'pig_test.txt',
     'translated_pig_test.txt',
     'Level 2 (Modern Python Scripts).ipynb',
     'Level 1 (Basics of Programming & Python).md',
     'Planning.md',
     'story.txt',
     'pets.txt',
     'Level 2 Exercises.ipynb']



We've managed to get rid of our `file.close()` line, but didn't I just make a big deal about how important that was? Well, the magic of the `with` block is that makes sure to close our file for us!

You can see that a `with` block starts with the `with` keyword, followed by the `open()`ing of a file. We then give a name to this newly opened file after the `as` keyword. Like with our other blocks (`if`, `while`, `for`, etc), we have a colon (`:`) followed by some number of indented lines. The secret sauce of `with` is that `file` is open for the duration of this block, but is automatically closed when we leave the indented region. The `with` block can also be used to manage a number of other resources; see [here for more](https://alysivji.github.io/managing-resources-with-context-managers-pythonic.html) about these "context managers".

With that out of the way, let's finally get to reading a file!


```python
# Try to open epic_story.txt for reading and writing (note the 'r+' mode)
with open('epic_story.txt', 'r+') as file:
    contents = file.read()

print('Hello there! Have a file:')
print(contents)
```


    ---------------------------------------------------------------------------

    FileNotFoundError                         Traceback (most recent call last)

    <ipython-input-248-41a22734dcc0> in <module>
          1 # Try to open epic_story.txt for reading and writing (note the 'r+' mode)
    ----> 2 with open('epic_story.txt', 'r+') as file:
          3     contents = file.read()
          4 
          5 print('Hello there! Have a file:')


    FileNotFoundError: [Errno 2] No such file or directory: 'epic_story.txt'


Well that's a bit rank, isn't it? Our code gets to the `open()` function, then keels over dead... What gives? What's happened here is that the `epic_story.txt` file doesn't actually exist yet! That's what Python was complaining about with the `FileNotFoundError`. While it might make sense for some errors to take down the whole program, it seems a bit silly here. Is there some way we could teach Python to handle these sort of errors on its own? Sure there is! We just need to tell Python to...

# Try Harder!

Alas, even highly motivated computers are still dumb. Let's teach it a thing or two about responsibility with a `try` block:


```python
try:
    # Change 'epic_story.txt' to 'story.txt' and see what happens!
    with open('epic_story.txt', 'r+') as file:
        contents = file.read()
except FileNotFoundError as err:
    contents = 'The file was empty...'
    print('Python made a stinky: ', err)
    
print('Hello there! Have a file:')
print(contents)
```

    Python made a stinky:  [Errno 2] No such file or directory: 'epic_story.txt'
    Hello there! Have a file:
    The file was empty...


In Python, we can wrap error-prone code in a `try` block (opened by `try:`), then gracefully handle any errors in the provided `except` clauses. Let's take a closer look at that except clause...

`except FileNotFoundError as err:`

The `except` keyword must follow a `try` block, and tells Python what to do with particular errors. You can specify which error you'd like to handle right after the `except`; in this case, that's `FileNotFoundError`. If you'd like more information about the error, you can use `as` to give it a name (`err` in this case).

When the above code is run with `'epic_story.txt'` in the `open()`, it throws an error that would normally crash the program, but since it was run in a `try` block, it looks for `except` clauses that match the error type. In this case, the `FileNotFoundError` matches, so Python runs the code in that block – setting `contents` to `'The file was empty...'` and printing an error without crashing the program.

Note that this code only handles `FileNotFoundError`s at the moment, so other errors could still crash our program:


```python
try:
    # The AI killer:
    bomb = 42 / 0
    with open('epic_story.txt', 'r+') as file:
        contents = file.read()
except FileNotFoundError as err:
    contents = 'The file was empty...'
    print('Python made a stinky: ', err)
    
print('Hello there! Have a file:')
print(contents)
```


    ---------------------------------------------------------------------------

    ZeroDivisionError                         Traceback (most recent call last)

    <ipython-input-250-ab077397b387> in <module>
          1 try:
          2     # The AI killer:
    ----> 3     bomb = 42 / 0
          4     with open('epic_story.txt', 'r+') as file:
          5         contents = file.read()


    ZeroDivisionError: division by zero


Fixing this, however, is easy enough. You could either add a dedicated `except` clause for `ZeroDivisionError`, or just catch everything with a "bare" `except` clause:


```python
try:
    # The AI killer:
    bomb = 42 / 0
    with open('epic_story.txt', 'r+') as file:
        contents = file.read()
except FileNotFoundError as err:
    contents = 'The file was empty...'
    print('Python made a stinky: ', err)
except: # This catches *all* errors... Use it sparingly!
    contents = "???"
    print('Something else went wrong...')
    
print('Hello there! Have a file:')
print(contents)
```

    Something else went wrong...
    Hello there! Have a file:
    ???


Above we've needed to define `contents` in every branch, even the exceptions, because it needs to be defined for `print(contents)` to run without error. Couldn't we only `print(contents)` when opening the file was a success? Sure we can! We just need an `else:` clause after all of the `except` ones!


```python
try:
    # Change 'epic_story.txt' to 'story.txt' and see what happens!
    with open('epic_story.txt', 'r+') as file:
        contents = file.read()
except FileNotFoundError as err:
    print('Python made a stinky: ', err)
else:
    print('Hello there! Have a file:')
    print(contents)
```

    Python made a stinky:  [Errno 2] No such file or directory: 'epic_story.txt'


Looking good! For completeness, there is also a `finally` clause that runs regardless of failure or success – even in the event of an unhandled failure!


```python
try:
    # Uncomment the bomb for destruction
    # bomb = 42 / 0
    # Change 'epic_story.txt' to 'story.txt' and see what happens!
    with open('epic_story.txt', 'r+') as file:
        contents = file.read()
except FileNotFoundError as err:
    print('Python made a stinky: ', err)
else:
    print('Hello there! Have a file:')
    print(contents)

finally:
    print('Code for cleaning up goes here!')
```

    Python made a stinky:  [Errno 2] No such file or directory: 'epic_story.txt'
    Code for cleaning up goes here!


Try uncommenting the `bomb = 42 / 0` line above. Even though the error still crashes the program, the cleanup code runs first!

# Say it short

I've made sure to save the best feature for last. It's time for our introduction to "f-strings" (formatted strings)! Let's start with a simple example:


```python
name = input('What is your name? ')
colour = input('What is your favourite colour? ')
# Lame non-f-string:
print('Hello {name}! What a coincidence, {colour} is my favourite colour too!')
# Cool, magical f-string
print(f'Hello {name}! What a coincidence, {colour} is my favourite colour too!')
```

    What is your name? Brooks
    What is your favourite colour? blue
    Hello {name}! What a coincidence, {colour} is my favourite colour too!
    Hello Brooks! What a coincidence, blue is my favourite colour too!


That's a bit funky! By putting an `f` before the start of our string, we've flipped on some new features! In f-strings, anything surrounded by curly braces (`{}`) is treated as Python code to be run. In the example above, we were just splicing in some variables, but we could get a bit fancier:


```python
name = input('What is your name? ')
colour = input('What is your favourite colour? ')
# Cool, magical f-string
print(f"Oh, it's {name[::-1].capitalize()}... {colour.upper()} is my least favourite colour...")
```

    What is your name? Isaac
    What is your favourite colour? purple
    Oh, it's Caasi... PURPLE is my least favourite colour...


Also, don't worry too much about that `name[::-1]` business, it's just a slice with a step (a bit like `range` with a step). Take a look at the code below to get a feel for what's going on:


```python
string = "I'm a string full of slices!"
# A normal slice
print(string[6:12])
# Every other letter
print(string[6:12:2])
# Backwards (negative step)
print(string[11:5:-1])
# Leaving out a start and end slices the whole string
print(string[:])
# Which can also have a step
print(string[::2])
# And be reversed
print(string[::-1])
```

    string
    srn
    gnirts
    I'm a string full of slices!
    Imasrn ulo lcs
    !secils fo lluf gnirts a m'I


Well sure, running code inside of your strings is kinda cool and pretty useful, but I'm greedy – I want more! Luckily, f-strings are packed full of other features like rounding numbers, automatic padding, justification, [and more](http://zetcode.com/python/fstring/)! I'd recommend checking out the documentation [here](https://docs.python.org/3/tutorial/inputoutput.html#formatted-string-literals) and [here](https://docs.python.org/3/library/string.html#formatspec), but I'll show off a couple of useful tidbits now:


```python
import math
print(f'Full pi: {math.pi}; A slice of pi: {math.pi:.2f}')
print(f'Slide to the |{"right!":>20}|')
print(f'Slide to the |{"left!":20}|')
print(f'Slide to the |{"middle?":^20}|')
print(f'I cast a hex on you! 0x{3735928559:X}')
```

    Full pi: 3.141592653589793; A slice of pi: 3.14
    Slide to the |              right!|
    Slide to the |left!               |
    Slide to the |      middle?       |
    I cast a hex on you! 0xDEADBEEF


# Bringing it all together

To wrap things up, let's head back to the pet shop and write some code that saves our pet records in a file for safe-keeping. We'll start with just printing the information to the screen and go from there.


```python
pets = [
    { 'name': 'fido', 'type': 'dog', 'age': 3 },
    { 'name': 'snowball', 'type': 'cat', 'age': 8 },
    { 'name': 'bubbles', 'type': 'fish', 'age': 2 },
]

for pet in pets:
    print(pet)
```

    {'name': 'fido', 'type': 'dog', 'age': 3}
    {'name': 'snowball', 'type': 'cat', 'age': 8}
    {'name': 'bubbles', 'type': 'fish', 'age': 2}


So far, so good! We have a list of our pets, each described with a dictionary, then we are looping through them with a `for` loop. With that being said, it would be nice if we could show that information in a more human-friendly way. Let's use an f-string to build some sentences!


```python
pets = [
    { 'name': 'fido', 'type': 'dog', 'age': 3 },
    { 'name': 'snowball', 'type': 'cat', 'age': 8 },
    { 'name': 'bubbles', 'type': 'fish', 'age': 2 },
]

for pet in pets:
    print(f"{pet['name'].capitalize()} is a {pet['type']} who is {pet['age']} years old.")
```

    Fido is a dog who is 3 years old.
    Snowball is a cat who is 8 years old.
    Bubbles is a fish who is 2 years old.


Hey, that's looking pretty good! Much easier on the eyes! Let's save it to a file:


```python
pets = [
    { 'name': 'fido', 'type': 'dog', 'age': 3 },
    { 'name': 'snowball', 'type': 'cat', 'age': 8 },
    { 'name': 'bubbles', 'type': 'fish', 'age': 2 },
]

# Writing our sentences to the file:
with open('pets.txt', 'w') as file:
    for pet in pets:
        file.write(f"{pet['name'].capitalize()} is a {pet['type']} who is {pet['age']} years old.\n")

# Reading our file:
with open('pets.txt') as file: # The default mode is 'r'
    print(file.read())
```

    Fido is a dog who is 3 years old.
    Snowball is a cat who is 8 years old.
    Bubbles is a fish who is 2 years old.
    


# That's All, Folks!

That was quite the whirlwind tour of Python, but you should now have everything you need to write maintainable, readable, Python scripts! Don't forget to try this week's exercises (now separated into their own worksheet), and ask for help on the Discord if you get stuck! Good luck! 

# Level 2 Exercises

Put your newfound skills to the test! Try to get as far as you can on your own, but some [model solutions can be found here](https://gitlab.com/sheffield-bionics/bootcamp-resources/-/blob/master/Python/level2.py).

## Important Resources
### Getting help and feedback

Even if you are going through this worksheet outside of a scheduled session time, you can always find help on the [Sheffield Bionics Discord](https://discord.gg/N4k7ECt)! Just drop a message in the #programming channel or directly message one of our coding instructors.

### Tools for writing Python code

For this course, we'll be working primarily in [Google Colaboratory](https://colab.research.google.com) and [repl.it](https://repl.it/) so that you don't need to install anything on your computer. If, however, you are interested in installing Python locally on your computer, you can find [detailed instructions here](https://realpython.com/installing-python/). You may also be interested in something like [Spyder](https://www.spyder-ide.org/) or [PyCharm](https://www.jetbrains.com/pycharm/) which make writing Python scripts a bit nicer.

# Guessing Game

Write a **function** implementing the following game:

1. Computer picks a random number 1-100
2. User guesses a number
3. Computer says if the guess is too high or too low
4. Repeat steps 1-3 until the user guesses correctly or 7 guesses have been made
5. If the user guesses correctly, print 'you win'
6. If 7 guesses aren't enough to guess the number, print 'you lose'

An example game might look as follows:

    >>> guessing_game()
    I'm thinking a of a number between 1 and 100... Can you guess it?
    Your guess: 50
    Sorry, that guess was too low!
    You have 6 guesses left...
    Your guess: 75
    Sorry, that guess was too low!
    You have 5 guesses left...
    Your guess: 87
    Sorry, that guess was too high!
    You have 4 guesses left...
    Your guess: 81
    Sorry, that guess was too high!
    You have 3 guesses left...
    Your guess: 78
    Sorry, that guess was too high!
    You have 2 guesses left...
    Your guess: 76
    Sorry, that guess was too low!
    You have 1 guesses left...
    Your guess: 77
    Well done! The secret number was 77!


```python
# Write your code here! (Or on repl.it)
```

# Word Reverser

Write a function that performs the following:

1. Take a sentence as an argument
2. Split it into words
3. Reverse each world individually
4. Returns the sentence with each word reversed

An example would be the following:

    >>> word_reverser('the quick brown fox jumps over the lazy dog')
    'eht kciuq nworb xof spmuj revo eht yzal god'

### Some Hints:

- Take a look at some of the [built-in string methods](https://docs.python.org/3/library/stdtypes.html#str.split) that might help with step 2
- For sticking words back together, you may find [this method](https://docs.python.org/3/library/stdtypes.html#str.join) useful.


```python
# Write your code here! (Or on repl.it)
```

# Pig Latin Translator

This one is really tricky! You've got this! Remember that you can always find help on the Discord!

Write a function that performs the following:

1. Take a filename as an argument
2. Read a text file into a string
3. Split it into words
4. If the first letter of the word is a vowel:
   1. Add 'way' to the end of the word
5. Otherwise:
   1. Move all letters before the first vowel to the end of the word
   2. Add 'ay' to the end of the word
6. Write the translated text into a new file

If you had this text in the file `pig_test.txt`:

    The most merciful thing in the world, I think, is the inability of the human mind to correlate all its contents. We live on a placid island of ignorance in the midst of black seas of infinity, and it was not meant that we should voyage far. The sciences, each straining in its own direction, have hitherto harmed us little; but some day the piecing together of dissociated knowledge will open up such terrifying vistas of reality, and of our frightful position therein, that we shall either go mad from the revelation or flee from the deadly light into the peace and safety of a new dark age.

Then you should be able to run this in Python:
    
    >>> pig_latin('pig_test.txt')

A new file called `translated_pig_test.txt` should be created with the following contents:

    eThay ostmay ercifulmay ingthay inway ethay orld,way Iway ink,thay isway ethay inabilityway ofway ethay umanhay indmay otay orrelatecay allway itsway ontents.cay eWay ivelay onway away acidplay islandway ofway ignoranceway inway ethay idstmay ofway ackblay eassay ofway infinity,way andway itway asway otnay eantmay atthay eway ouldshay oyagevay ar.fay eThay iences,scay eachway ainingstray inway itsway ownway irection,day avehay ithertohay armedhay usway ittle;lay utbay omesay ayday ethay iecingpay ogethertay ofway issociatedday owledgeknay illway openway upway uchsay errifyingtay istasvay ofway eality,ray andway ofway ourway ightfulfray ositionpay erein,thay atthay eway allshay eitherway ogay admay omfray ethay evelationray orway eeflay omfray ethay eadlyday ightlay intoway ethay eacepay andway afetysay ofway away ewnay arkday age.way

You may notice, however, the capital letters and punctuation end up a bit wonky. For an **extra challenge**, try fixing those and generating a file like this:

    Ethay ostmay ercifulmay ingthay inway ethay orldway, Iway inkthay, isway ethay inabilityway ofway ethay umanhay indmay otay orrelatecay allway itsway ontentscay. Eway ivelay onway away acidplay islandway ofway ignoranceway inway ethay idstmay ofway ackblay eassay ofway infinityway, andway itway asway otnay eantmay atthay eway ouldshay oyagevay arfay. Ethay iencesscay, eachway ainingstray inway itsway ownway irectionday, avehay ithertohay armedhay usway ittlelay; utbay omesay ayday ethay iecingpay ogethertay ofway issociatedday owledgeknay illway openway upway uchsay errifyingtay istasvay ofway ealityray, andway ofway ourway ightfulfray ositionpay ereinthay, atthay eway allshay eitherway ogay admay omfray ethay evelationray orway eeflay omfray ethay eadlyday ightlay intoway ethay eacepay andway afetysay ofway away ewnay arkday ageway.
    
This one's a stinker, so keep at it an ask for help if needed!


```python
# Write your code here! (Or on repl.it)
```
