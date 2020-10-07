---
title: "Basics of Programming & Python"
date: 2020-10-07T07:31:21+01:00
authors: ["tll"]
categories:
  - Software
tags:
  - Week 1
  - Level 1
  - Basics
  - Python
  - Worksheet
  - Programming
---

# Important Note

This is worksheet is meant to be interactive! I'd **highly recommend** you copy [this document](https://colab.research.google.com/drive/1y0UbIIXZJM_6kVTnEYq6eaUxPRL4Z3f_?usp=sharing) to your Google Drive and go through it in Google Colaboratory. This page is here primarily for archival purposes. If you are looking for example solutions to the exercises, you can find those [here](https://gitlab.com/sheffield-bionics/bootcamp-resources/-/blob/master/Python/level1.py).

# Basics of Programming & Python

## Why learn to program?

Computers are very powerful tools that have made their way into our homes, workplaces, laps, and pockets. While most people nowadays are comfortable using applications developed by others – things like Microsoft Word, Chrome, or even more complicated programs – there exists a shortage of computer programmers who actually build these applications.

Even if you're uninterested in software development, there are countless tedious tasks that can be either partially or totally automated by an ad hoc computer program. There are [entire books](https://automatetheboringstuff.com/) that walk through automating menial tasks such as sorting through files, updating spreadsheets, sending reminder emails, and more! With a bit of basic programming, you can save yourself from ever wasting time on repetitive work again.

Finally, many of the projects run by Sheffield Bionics rely on code to tie everything together; building a mechanically impressive arm packed with powerful motors is little good without the code telling it how to move.


## What actually is programming?

### Computers are kinda dumb

While it's become quite common to think of modern computers as "smart" devices, this is just a clever illusion constructed by software developers. Even the most powerful computers today are little more than fancy calculators.

Like a calculator, computers are quite good at quickly executing different instructions, but need a human telling them what to do when. A calculator can compute {{< katex inline >}} 2^{32} = 4294967296 {{< /katex >}} in the blink of an eye, but would never do anything if not for the button-pressing human.

### Okay computer, make me a sandwich!

That's a noble first attempt, I'll give you that, but computers aren't very good at understanding everyday English – they need very specific instructions. The problem computers have with English (or any other natural language) is its ambiguity. What type of sandwich should the computer make? Where can it find the bread? How many milliliters of mayo would you like?

To resolve this ambiguity and help programmers communicate precisely with computers, a smattering of man-made "programming languages" have been designed. Depending on how you count, there are 200-700 different programming languages in existence, with a [couple dozen](https://www.tiobe.com/tiobe-index/) being commonly used. Every language has it's own unique quirks, and it can be rather enlightening to explore some of the less popular ones, but for this course, we'll be learning the increasingly popular Python programming language.

### Okay, I'm hungry, let's get this show on the road!

Alright, alright... Just a minute... Before we get started, it's important that you know how to run the code in this notebook! Jupyter notebooks like this one are made of text and code cells. Code cells can be run by selecting them and pressing Ctrl/Cmd-Enter, or by pressing the play button. To edit a code cell, simply click where you'd like to type.

## Important Resources
### Getting help and feedback

Even if you are going through this worksheet outside of a scheduled session time, you can always find help on the [Sheffield Bionics Discord](https://discord.gg/N4k7ECt)! Just drop a message in the #programming channel or directly message one of our coding instructors.

### Tools for writing Python code

For this course, we'll be working primarily in [Google Colaboratory](https://colab.research.google.com) and [repl.it](https://repl.it/) so that you don't need to install anything on your computer. If, however, you are interested in installing Python locally on your computer, you can find [detailed instructions here](https://realpython.com/installing-python/). You may also be interested in something like [Spyder](https://www.spyder-ide.org/) or [PyCharm](https://www.jetbrains.com/pycharm/) which make writing Python scripts a bit nicer.

# Python as a calculator

Right, with all of that talk of computers just being fancy calculators, it would be pretty embarrassing if you couldn't add numbers with it. Luckily, Python has got us covered! Go ahead and run the code below and see what it returns. Feel free to change the code and play with it a bit!


```python
2 + 2
```




    4



No need to hide your excitement, this is thrilling stuff! As you can probably tell, Python is designed to be quite intuitive and maths is no exception. Many of the other operations are just as intuitive, but some of the symbols used may be new to you. Here is an overview of the four basic arithmetic operators:


```python
# Addition
2 + 2
```




    4




```python
# Subtraction
5 - 3.5
```




    1.5




```python
# Multiplication (with an asterisk)
3.14 * 2
```




    6.28




```python
# Division (with a forward-slash)
20 / 4
```




    5.0



Those wacky lines starting with `#` are called comments and are totally ignored by the computer. They are used to communicate to other human programmers what is going on in a piece of code. Write whatever you'd like in them!

There are also a couple more operators that might come in handy and are a bit strange looking:


```python
# Exponentiation (the same as 2 * 2 * 2 * 2)
2**4
```




    16




```python
# Modulo (returns the remainder of a division)
5 % 2
```




    1



The `**` operator lets you raise one number to the power of another and behaves like a repeated multiplication. 

The `%` is a bit strange and returns the remainder of a division – `5 % 2` is `1` because `5 / 2` is `2` with `1` left over. Don't worry about modulo (`%`) too much for now, it makes more sense in context later on.

Of course, Python is perfectly capable of handling more complex expressions, respecting the order of operations and any added brackets:


```python
(50 - 5*6) / 4
```




    5.0



# Can your calculator do this?!

So far we've seen that Python – a massive, international, 30 year old project – is capable of keeping up with your desk calculator. So far, so good, but can't we do something more with it? Certainly! Let's take a look at another type of data that Python can understand: strings!


```python
"I'm a string!"
```




    "I'm a string!"




```python
'So am I!'
```




    'So am I!'



Well, a far cry from lacing your shoes, these strings hold textual data. Though the terminology may be somewhat alien, you can think of strings as letters (or numbers or symbols) that have been _strung_ together in sequence. The letters / numbers / symbols that make up a string are called characters.

Strings in Python are wrapped in either single (`'`) or double (`"`) quotes and can contain just about anything you can type on a keyboard. You'll have to be a bit careful if you are trying to include other quotes in your string though; Python can get confused about where the string starts and stops:


```python
'I'm a load of trouble!'
```


      File "<ipython-input-11-ad43a587e7ac>", line 1
        'I'm a load of trouble!'
           ^
    SyntaxError: invalid syntax




```python
"And then I said: "Uh oh...""
```


      File "<ipython-input-12-9e7ae666966e>", line 1
        "And then I said: "Uh oh...""
                           ^
    SyntaxError: invalid syntax



To fix this, you can just switch the outer quote type or precede the offending quote(s) with a back-slash (`\`)


```python
"I'm a load of trouble!"
'I\'m a load of trouble!'
```




    "I'm a load of trouble!"




```python
'And then I said: "Uh oh..."'
"And then I said: \"Uh oh...\""
```




    'And then I said: "Uh oh..."'



Yuck... Let's get back out of the weeds and move on to something a bit more interesting! Before we dive into what you can do with strings, it's helpful to know that you can display them without quotes using the `print` function. You can also use `print` several times to display more than one result.


```python
print("To the icky quotes I say: \"Be gone!\"")
print('This is nicer than my crappy inkjet...')
```

    To the icky quotes I say: "Be gone!"
    This is nicer than my crappy inkjet...


To call a function like `print` (and indeed all Python functions), you add brackets / parentheses (`()`) after the function name. For functions that take additional data, that's passed to the function within the brackets and separated by commas (`,`).


```python
print('My favourite number is:', 42, '!')
```

    My favourite number is: 42 !


We will dive *much* more deeply into functions in the future, but for now you can just think of them as bits of code that have been bundled up for easy reuse elsewhere – perhaps something akin to a fancy document template. Let's continue playing with strings some more!


```python
# Strings can be concatentated (stuck together) with the addition (+) operator
'Ham' + ' and ' + 'cheese'
```




    'Ham and cheese'




```python
# Strings can be repeated with the multiplication (*) operator
'un' * 3 + 'ium'
```




    'unununium'



Hey, that's pretty fun! Or as a Python programmer might say: `'Pretty C' + 'o' * 10 + 'l'`.

What if you'd like to just take a look at some of the letters? Say, the second letter in `'Hello!'`. Helpfully, Python lets us "index" strings and fetch a particular character (letter) from it. Let's try to fetch the second character!


```python
# Indexing is done with square brackets and a number between them
'Hello!'[2]
```




    'l'



Uh oh... Something went wrong there... The second letter should be `'e'`! As it turns out, Python starts counting from 0, not from 1. This means that `'H'` is actually at index 0, and `'e'` is at index 1. Let's give that a try now!


```python
'Hello!'[1]
```




    'e'



That's much better! This counting from zero may seem a little counter-intuitive at first, but is actually quite common in programming. We'll see later on that this makes a number of common coding tasks a bit easier.

Let's take a look a couple more tricks that indexing has up its sleeve!


```python
# Negative indexing counts from the back of the string and starts at -1
print('Hello!'[-1])
print('Hello!'[-2])
print('Hello!'[-3])
```

    !
    o
    l



```python
# Indexing with a range is called slicing
print('Hello!'[1:5])
print('Hello!'[0:2])
```

    ello
    He


Whoa, what's up with that slicing business? Slicing lets you extract a range of characters from a string. The first index is included in the slice, but the last index is *excluded*. That's why `'Hello!'[0:2]` gives us `He` and not `Hel` (recall that `'Hello!'[2]` is `l`).

If you leave out an index, the slice extends to either the beginning or the end of the string:


```python
# Slicing has helpful defaults if you leave out an index
print('Hello!'[3:])
print('Hello!'[:4])
```

    lo!
    Hell


Now that's one heck of a calculator! These tricks with strings will come in handy in the future when we are picking apart text and writing new messages!

Oh, and before moving on to lists, it can be helpful to know how long a string is! No, you can put away your ruler... For this, Python has the `len()` function.


```python
# The answer is: too long...
len('supercalifragilisticexpialidocious')
```




    34



# Working with more than one bit of data

Back when we were first getting started, I made quite the point of computers being good at repetitive tasks, working on many copies of a similar thing, but how do computers keep track of all that data? In Python, the answer is lists! Let's take a look at our first Python list:


```python
['Welcome to', 42, 'List', 3.14, 'Land!']
```




    ['Welcome to', 42, 'List', 3.14, 'Land!']



That's not too bad, lists are just a collection of values between square brackets and separated by commas. Hmm, what if I wanted to pull `42` out of that list? Let's try something familiar...


```python
['Welcome to', 42, 'List', 3.14, 'Land!'][1]
```




    42



Hey! That's not too bad! It's just indexing all over again! With that being said, that's starting to look pretty ugly... Fortunately for us, Python lets us store data like this list in little named boxes called variables. You can assign some data to a variable with the equals sign (`=`).


```python
# Storing our list in a variable named `my_list`
my_list = ['Welcome to', 42, 'List', 3.14, 'Land!']
```


```python
# Taking a peek at what is inside our `my_list` box
my_list
```




    ['Welcome to', 42, 'List', 3.14, 'Land!']



If you run into a problem running that second cell, but sure that you've run the first one! Now with our list safely stowed away in a variable, getting the `42` back out looks much nicer!


```python
my_list[1]
```




    42



Helpfully for us, lists have nearly all of the same powers that strings do. In addition to indexing, they are down for:


```python
# Concatenation
[1, 2] + [3, 4, 5]
```




    [1, 2, 3, 4, 5]




```python
# Repetition
['Beep', 'Boop'] * 2 + ["I'm a robot"]
```




    ['Beep', 'Boop', 'Beep', 'Boop', "I'm a robot"]




```python
# Slicing
lst = ['Slice me', 'Dice me', 'Chop me', 'Drop me']
print(lst[:2])
print(lst[1:3])
print(lst[-2:])
```

    ['Slice me', 'Dice me']
    ['Dice me', 'Chop me']
    ['Chop me', 'Drop me']



```python
# And length checking
len([1,2,3,4,5,6,7,8,9,0])
```




    10



That's not half bad! But what if I want to change some part of my list? What if I want to add new things to it? Python treats each index like a sort of variable box, so we can use `=` again to put something new in one of the boxes.


```python
lst = ['This', 'list', 'has', 'something', 'old!']
print(lst)
lst[-1] = 'new!'
print(lst)
```

    ['This', 'list', 'has', 'something', 'old!']
    ['This', 'list', 'has', 'something', 'new!']


That was pretty neat! We opened the last box of the list at index -1, then put a new value in its place. But what if I'd like to add something new to the end of list? Maybe indexing will help us here?


```python
lst[6] = 'How fun!'
```


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    <ipython-input-35-d7304c75e490> in <module>
    ----> 1 lst[6] = 'How fun!'
    

    IndexError: list assignment index out of range


Oh dear, that wasn't quite right... Python is scolding us for trying to open an index that isn't present in our list! We'll have to add on to our list in some other way. In this case, Python provides a *method* for appending items on to the end of a list. Let's take a look:


```python
lst.append('How fun!')
lst
```




    ['This', 'list', 'has', 'something', 'new!', 'How fun!']



Let's try to break down this newfangled "method" thing. A method is a special type of function that stored alongside a piece of data. In this case, the `.append()` method is stored with the list data. You can call these methods by first writing out the data (or the name of a variable containing it) followed by a dot (`.`), then the method name. Arguments to the method are given between the brackets and separated by commas – just as with functions.

Different methods are available for different types of data. Strings, for example, have the `.upper()` method for converting everything to uppercase. Let's take a look:


```python
'Just a normal string'.upper()
```




    'JUST A NORMAL STRING'



Since methods are tightly tightly coupled with the data before the dot, they can access it even if it's not between the brackets. Above, we saw the `.upper()` method capitalize the string that came before the dot.

Methods are important to a style of programming called "Object Oriented" programming, but that's a more advanced topic we'll revisit later, so don't worry if the distinction between methods and functions is still a little muddy. For now, let's move past playing around with these snippets of data and on to some real programming!

# Writing Programs

As you may have picked by now, Python can run multiple lines of code at a time and executes the lines from top to bottom. While we've been playing with mostly quite small examples so far, as your programs become more complex, you'll want to save them in a Python script so they can be rerun later without a human needing to copy-paste it into a Python interpreter.

Jupyter code cells are a sort of hybrid between a script and an interpreter. The last line of code in a Jypyter cell generally acts as if it were typed into a Python interpreter because the *return value* of that code is printed automatically. The lines before the last line act more like a script and unused returns are discarded.

For example, as we've learned, `2 + 2` *returns* 4 and `4 * 5` *returns* 20. If, however, we put both in a Jupyter cell together:


```python
# Only the return value of the last line is printed
2 + 2
4 * 5
```




    20



The distinction between working in an interpreter and a script is muddied quite a bit by Jupyter, but is made much clearer when working in something like [Spyder](https://www.spyder-ide.org/) or [repl.it](https://repl.it). Don't worry too much if this is fuzzy right now.

While scripts saved in files move us away from repetitive typing, they also make it a bit harder to communicate with the computer – it's easy to change the code while you are copying it into an interpreter, but you generally don't want to rewrite parts of your script every time you run it.

How would you change the name of the person this program greets if you couldn't edit the code?


```python
# How would you greet someone else without changing this code?
name = 'Brooks'
print('Hello', name)
```

    Hello Brooks


In this case, Python provides us a sort of inverse of the `print()` function: the `input()` function. While the `print()` function allows Python to talk to the programmer, the `input()` function is our way of talking to Python. Let's take a look at how it works! Press enter after you've typed in a name.


```python
name = input('Who would you like to greet? ')
print('Hello', name)
```

    Who would you like to greet? Brooks
    Hello Brooks


The `input()` function optionally takes a prompt message, which is printed to the screen, and *returns* whatever the user typed in. In the above example, the prompt message is `'Who would you like to greet? '` and the user's answer is stored in the `name` variable.

`input()` can also be run without an argument (the thing between the brackets). In this case, it prints nothing and waits for user input. 


```python
input()
```

    Bruh





    'Bruh'



# Making Choices

Having a way to talk to the computer is great, but quickly presents us with a new problem: we can change variables, but we can't change what code is run in our scripts. What if we wanted to write a program that asked you for a type of sandwich and ran different sandwich-making code depending on your answer? It's easy to type different code into an interpreter, but then we are back to doing things manually.

In Python, there is a better way: `if-else` statements. Let's take a look at one and examine it piece by piece.


```python
ans = input('Would you like a sandwich? ')
if ans == 'yes':
    print("I'll get on it right away!")
else:
    print('Will I never please you?')
```

    Would you like a sandwich? no
    Will I never please you?


At the moment, this script is quite particular, so if you are typing 'yes', be sure that it's all lower-case and there are no extra spaces.

Let's look at things line by line:

`ans = input('Would you like a sandwich? ')`

So far, so good! This is familiar and is asking a question before waiting for user input and storing the user's response in the `ans` variable. On to the next!

`if ans == 'yes':`

Ah, there is a lot of new stuff here! First of all is the `if` keyword. This `if` is followed by some sort of condition (something that can be either true or false) and then a colon (`:`). Here, the condition is `==`. While a single equal sign is assignment – what we've been using to store values in variables – a double equal sign (`==`) tests if two values are actually equal. Something like `2 == 2` is `True`, while `2 == 1` is `False`. Here, this is checking if the user typed `yes` in response to the sandwich question. If they did type `yes`, then `ans == 'yes'` returns `True` and Python goes on to run the indented code after the colon.

`    print("I'll get on it right away!")`

This is a mostly familiar print statement, but you'll notice it's been indented. This means that it belongs to the first branch of the `if` statement and, consequentially, is only run by Python when `ans == 'yes'` is `True`.

`else:`

This rather short line is the other half of the `if` condition. The code indented after this `else` line is only run when `ans == 'yes'` is **`False`**

`    print('Will I never please you?')`

This indented print statement belongs to the `else` block and is run if `ans` is anything *other than* `yes`.

Note that the indented sections can be of any length and that the `else` block is optional! The code below shows off an indented block 3 lines long without an `else` clause. Change `True` to `False` and see how the behavior compares. The condition only controls indented code, so anything that's unindented will run normally as if it weren't there.


```python
if True:
    print('Some code')
    print('A bit more')
    print('How about another')
print('I always run!')
```

    Some code
    A bit more
    How about another
    I always run!


Occasionally you need to check for more than just one condition before falling back to an else clause. When you've got several choices to make like this, you can use `elif` (short for "else-if"). Try changing the value of `x` below.


```python
x = 5
if x > 10:
    print('x is too big to handle!')
elif x < 1:
    print('x is pathetically small...')
else:
    print('x is of a respectable size')
```

    x is of a respectable size


Conditions are tried one at a time from the top down – the `elif` block only runs if the `if` block doesn't, and the `else` block only runs if neither the `if` nor the `elif` blocks do. You can string together as many `elif` blocks as you'd like; you aren't limited to one, like we've used here.

Conditions can also be combined using the `and` and `or` keywords. If two conditions are joined with `and`, then both must be `True` for the result to be `True`. If two conditions are joined with `or`, then only one needs to be `True`. Try to predict what some of the combined conditions below evaluate to:


```python
2 < 3 and 3 < 4
```




    True




```python
2 < 3 and 3 < 3
```




    False




```python
2 < 3 or 3 < 4
```




    True




```python
2 < 3 or 3 < 3
```




    True




```python
2 < 2 or 3 < 3
```




    False



You can also flip `True` and `False` with the `not` keyword:


```python
not True
```




    False




```python
not 1 == 2
```




    True



# Looping back

It's finally time to piece all of this together and make good on my promise to get rid of needless repetition. If you want to run some code repeatedly in Python, you can use a `while` loop. These look somewhat like `if` statements, but behave a bit differently.


```python
while input('Sandwich, my liege? ') == 'yes':
    print('Please enjoy your wich of sand')
```

    Sandwich, my liege? yes
    Please enjoy your wich of sand
    Sandwich, my liege? yes
    Please enjoy your wich of sand
    Sandwich, my liege? yes
    Please enjoy your wich of sand
    Sandwich, my liege? no, yuk


Try answering `yes` several times. To break out of the loop, type anything else.

Similar to an `if` statement, a `while` loop takes a condition (which is `input('Sandwich, my liege? ') == 'yes'` here) and an indented block of code. The difference here is that the block of code after the `while` is run over and over as long as the condition is `True`. Once the condition becomes `False`, Python stops looping and resumes normal line-by-line execution.

Putting a couple of things together, we can use loops to do things like basic counting. Let's take a look at another example, then break it down.


```python
x = 0
while x <= 10:
    print('x is:', x)
    x = x + 1
```

    x is: 0
    x is: 1
    x is: 2
    x is: 3
    x is: 4
    x is: 5
    x is: 6
    x is: 7
    x is: 8
    x is: 9
    x is: 10


`x = 0`

This is pretty familiar by now, we're storing 0 in the `x` variable.

`while x <= 10:`

The new bit here is `<=`. While `==` tests if two values are equal, `<=` tests if the first value is less than or equal to the second. In example form: `9 <= 10` is `True`, and so is `10 <= 10`, but `11 <= 10` is `False`. Ultimately, this loop will continue until x is greater than 10.

`    print('x is:', x)`

Part of the loop, this prints what `x` is each time the loop repeats.

`    x = x + 1`

This final line updates the value of `x`. It takes the current value of `x`, adds 1 to it, then sets `x` to that new value. This adds 1 to `x` every time the loop is run. This `x = x + ...` pattern is common enough that Python provides a shorthand version: `x += 1` means the exact same thing as `x = x + 1`. This shorthand also exists for the other operators (`-=`, `*=`, etc.)

# The Final Frontier

To pull together everything we've learnt, let's write a Python script that simulates a rocket launch countdown. We'll start by asking the user at what number to start the countdown, then count down from that number, printing each time the counter changes. At and under 5 seconds, we'll switch to printing messages in all capital letters (the excitement is a lot to handle), and once the countdown hits zero, we'll print a "blastoff" message and break out of the loop.

Let's start with something rather simple, just taking a number from the user and counting down. The `>` here is just testing if the first value is greater than the second – `2 > 1` is `True`, but `1 > 1` is `False`.


```python
x = input('Where should the countdown begin? ')
while x > 0:
    print(x)
    x -= 1
```

    Where should the countdown begin? 10



    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-54-2a0ba5dab1bd> in <module>
          1 x = input('Where should the countdown begin? ')
    ----> 2 while x > 0:
          3     print(x)
          4     x -= 1


    TypeError: '>' not supported between instances of 'str' and 'int'


Oh dear... That didn't go quite as planned... Our issue here is that `x` is a string, not a number! Even if you are sure to input a number, Python still sees `'10'` and `10` as different types of data. Luckily, it's possible to convert between types using a set of functions. `10` is a particular type of number called an integer. And integer is essentially a whole number without any decimal point. Something like `10` is an integer, but `3.14` is not!

We can convert strings to integers using the `int()` function; let's give that a try:


```python
x = int(input('Where should the countdown begin? '))
while x > 0:
    print(x)
    x -= 1
```

    Where should the countdown begin? 10
    10
    9
    8
    7
    6
    5
    4
    3
    2
    1


Looking good! Notice how we can nest the `input()` and `int()` functions. `input()` returns a string and, since it's within the brackets of `int()`, `int()` takes that string and converts it to an integer which is stored in `x`. Note also that `int()` throws a fit if you enter something that isn't an integer – we'll learn to recover from this error later on.

Let's take a couple of easy steps and add the "blastoff" and countdown messages.


```python
x = int(input('Where should the countdown begin? '))
while x > 0:
    print('Rocket is launching in', x, '...')
    x -= 1
print('BLASTOFF!!!')
```

    Where should the countdown begin? 10
    Rocket is launching in 10 ...
    Rocket is launching in 9 ...
    Rocket is launching in 8 ...
    Rocket is launching in 7 ...
    Rocket is launching in 6 ...
    Rocket is launching in 5 ...
    Rocket is launching in 4 ...
    Rocket is launching in 3 ...
    Rocket is launching in 2 ...
    Rocket is launching in 1 ...
    BLASTOFF!!!


Looking pretty good! All we need to do is get hyped under 5 seconds! We know we can use an `if` statement to check if the counter is at 5 seconds or below, so let's give this a swing...


```python
x = int(input('Where should the countdown begin? '))
while x > 0:
    if x <= 5:
        print('ROCKET IS LAUNCHING IN', x, '...')
    else:
        print('Rocket is launching in', x, '...')
    x -= 1
print('BLASTOFF!!!')
```

    Where should the countdown begin? 10
    Rocket is launching in 10 ...
    Rocket is launching in 9 ...
    Rocket is launching in 8 ...
    Rocket is launching in 7 ...
    Rocket is launching in 6 ...
    ROCKET IS LAUNCHING IN 5 ...
    ROCKET IS LAUNCHING IN 4 ...
    ROCKET IS LAUNCHING IN 3 ...
    ROCKET IS LAUNCHING IN 2 ...
    ROCKET IS LAUNCHING IN 1 ...
    BLASTOFF!!!


Awesome! That's a fully working countdown! There's just one problem... It's a tad repetitive around the `print` lines... In general, programmers try to follow the DRY principle: Don't Repeat Yourself. This makes it easier to change things in the future – if we wanted to change the countdown message right now, we would need to rewrite it in two places! Is there some way we could do this better? Well, we know we can use the `.upper()` method to capitalize strings... Maybe we could replace the first part of the print statement with a variable that contains our message? Let's give that a go:


```python
x = int(input('Where should the countdown begin? '))
while x > 0:
    msg = 'Rocket is launching in'
    if x <= 5:
        msg = msg.upper()
    print(msg , x, '...')
    x -= 1
print('BLASTOFF!!!')
```

    Where should the countdown begin? 10
    Rocket is launching in 10 ...
    Rocket is launching in 9 ...
    Rocket is launching in 8 ...
    Rocket is launching in 7 ...
    Rocket is launching in 6 ...
    ROCKET IS LAUNCHING IN 5 ...
    ROCKET IS LAUNCHING IN 4 ...
    ROCKET IS LAUNCHING IN 3 ...
    ROCKET IS LAUNCHING IN 2 ...
    ROCKET IS LAUNCHING IN 1 ...
    BLASTOFF!!!


That's much better! We are back to only having one message and one print statement. It's super easy to update the message in the future, should we ever need to! Here we are always running the `print(...)` code, but the `msg` variable can contain either the lowercase or uppercase versions of the message. When `x` is below 5, we replace the current value of `msg` with the uppercase version returned by `msg.upper()`.

# Practice Makes Perfect

That's all of the new content for this worksheet, but I've prepared a number of coding challenges for you to put your skills to work! Don't worry about finishing all of these now, they are here for you to work on in your own time. With that being said, you're not alone either. If you have any questions at all, don't hesitate to ask on the Discord or during a session.

There is almost always something new to learn from others, so if you'd like, you can share your solutions with others or one of our mentors for some feedback! I'll always be around the Discord to offer detailed feedback or provide hints, if you need any.

# Factorial Calculator

[Factorial](https://en.wikipedia.org/wiki/Factorial) is a mathematical function you may have heard of before. It's represented with an exclamation point (`!`) and is calculated as follows:

$5! = 5 \cdot 4 \cdot 3 \cdot 2 \cdot 1 = 120$

Write a script that asks the user for some number, then uses a loop to calculate the factorial of that number. If the user inputs 5, you should output 120; if they enter 3, you should output 6; etc.


```python
# Write your code here! (Or on repl.it)
```

# FizzBuzz

The classical coding interview question, write a program that does the following:

1. Build a list of numbers 1-100
2. Replace multiples of 3 with 'Fizz'
3. Replace multiples of 5 with 'Buzz'
4. Replace multiplies of both with 'FizzBuzz'

Hint: 20 is a multiple of 5 because 20 / 5 = 4 without any remainder left over – the remainder is 0. Perhaps you know of an operator that returns the remainder of a division?

As an example, the first 15 items of the list should be:

```
[1, 2, 'Fizz', 4, 'Buzz', 'Fizz', 7, 8, 'Fizz', 'Buzz', 11, 'Fizz', 13, 14, 'FizzBuzz'...]

```


```python
# Write your code here! (Or on repl.it)
```

# Tripoint Artist

In this challenge, you should "draw" some shapes by printing text. More specifically, you should ask the user to input the height of a triangle to draw, and your script should draw it with `#` characters.

The first challenge is to draw a right triangle that looks like this:

```
Triangle height: 5
#
##
###
####
#####
```

For an additional challenge, try drawing pyramids that look like this:
```
Pyramid height: 5
    #
   ###
  #####
 #######
#########
```

Your program should be capable of drawing these shapes at any size given by the user:
```
Triangle height: 2
#
##

Pyramid height: 8
       #
      ###
     #####
    #######
   #########
  ###########
 #############
###############

etc...

```


```python
# Write your code here! (Or on repl.it)
```
