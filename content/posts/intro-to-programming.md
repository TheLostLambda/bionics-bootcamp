---
title: "Programming in Python Syllabus"
date: 2020-09-11T14:20:36+01:00
authors: ["tll"]
categories:
  - Software
tags:
  - Week 1
  - Week 2
  - Basics
  - Python
  - Programming
---

Learning Objectives
===================

1.  Understand how computer programming can be applied to simple
    problems
2.  Be able to express simple pseudo-code algorithms in Python
3.  Be familiar with Python\'s tools for writing maintainable and
    reusable code

Delivery
========

Lectures
:   Done either in-person or via video for levels 1 & 2. They will be
    quite interactive, often polling the audience for questions,
    suggestions, and answers. These should last around 1 hour.

Jupyter Notebooks
:   One for every level, containing both the topics taught and the
    post-lesson exercises. They should serve as a stand-alone learning
    resource.

Tutorial Sessions
:   These complement the Jupyter Notebooks and make up the optional
    second hour of each session. Students work largely independently on
    the exercises or on an applied project. Mentors will be available to
    provide guidance and answer questions.

Digital Tools & Platforms
-------------------------

### Lectures

-   <https://discord.com/>
-   <https://repl.it/>

### Jupyter Notebooks

-   <https://colab.research.google.com>

Content
=======

Level 1 (Programming & Python Basics)
-------------------------------------

### Topics

-   Why Learn to Program? \[10m\]
-   Python Interpreter \[20m\]
    -   Numbers
    -   Strings
    -   Lists
-   Writing Programs \[30m\]
    -   Scripts
    -   Input & Output
    -   Conditionals
    -   Looping

### Exercises

-   Factorial
    1.  Given n by the user, calculate n!
-   Triangle
    1.  Draw an n-height right, isosceles triangle with \'\#\' and
        print()
    2.  Draw an n-height pyramid with \'\#\' and print()
-   FizzBuzz
    1.  Print numbers 1-100
    2.  Replace multiples of 3 with \'Fizz\'
    3.  Replace multiples of 5 with \'Buzz\'
    4.  Replace multiplies of both with \'FizzBuzz\'

Level 2 (Modern Python Scripts)
-------------------------------

### Topics

-   Modules \[10m\]
    -   Import statements
    -   Running `__main__`
    -   Command-line arguments
-   Fancier Data \[15m\]
    -   List methods
    -   Dictionaries
    -   `del` and `in`
-   Fancier I/O \[10m\]
    -   Format strings
    -   Files
-   Fancier Control \[10m\]
    -   Try-catch
    -   Range
    -   Unpacking
-   Functions \[15m\]
    -   Rationale
    -   Argument types
    -   Return values
    -   Documentation strings

### Exercises

-   Guessing Game
    1.  Computer picks a random number 1-100
    2.  User guesses a number
    3.  Computer says if the guess is too high or too low
    4.  Repeat 1-3 until the user guesses correctly or 7 guesses have
        been made
    5.  If 7 guesses aren\'t enough to guess the number, print \'you
        lose\'
-   Word Reverser
    1.  Take a sentence from the user
    2.  Split into words
    3.  Reverse each world individually
    4.  Print out the sentence with each word reversed
-   Pig Latin Translator
    1.  Read a text file into a string
    2.  Split into words
    3.  If the first letter is a vowel:
        1.  Add \'way\' to the end of the word
    4.  Otherwise:
        1.  Move all letters before the first vowel to the end of the
            word
        2.  Add \'ay\' to the end of the word
    5.  Write the translated text into a new file

Level 3 (Object-oriented Programming)
-------------------------------------

### Topics

-   The Object-Oriented Abstraction \[15m\]
    -   Classes as blueprints
    -   Objects as grouped data & functionality
    -   Inheritance
    -   Why use it?
-   Classes and Objects \[25m\]
    -   Constructors
    -   Documentation strings
    -   Instance variables
    -   Class variables
    -   Instance methods
    -   Class methods
-   Exploring Inheritance \[20m\]
    -   Single inheritance
    -   Accessing the `super()` class
    -   Overriding methods
    -   Multiple inheritance

### Exercises

-   The Vehicle Class
    1.  Defined a new class called `Vehicle`
    2.  Define a constructor which takes a model name, fuel capacity,
        speed, and optionally mpg (mpg should default to 30).
        1.  Set the `model`, `fuel_cap`, `speed`, `mpg`, `fuel`, and
            `odometer` instance variables. `fuel` should initially equal
            `fuel_cap`, and `odometer` should start at 0
    3.  Implement a `drive(distance)` method where distance is some
        distance in miles.
        1.  If the fuel left in `self.fuel` is enough to drive the
            `distance`:
        2.  Decrement `self.fuel` by the amount used and increment
            `self.odometer` by the distance travelled
        3.  Then return the model of the vehicle, the distance driven,
            and the number of minutes it took (given it was travelling
            at `self.speed`)
        4.  If there wasn\'t enough fuel for the journey, return an
            error message with the model of the vehicle
    4.  Implement a `refuel()` method that sets `self.fuel` back to the
        max capacity, then prints how much fuel was added to top off the
        tank.
-   Extending the Vehicle Class with Class Methods
    1.  Add a class variable (shared by all instances) called
        `on_the_road` and set it to an empty list
    2.  Every time a new vehicle is constructed, it should append `self`
        to this list
    3.  Implement a class method called `count_models(model)` that takes
        a model name and returns the number of those models on the road.
        You should be able to call it like:
        `Vehicle.count_models('DeLorean')`
-   Inheritance
    1.  Implement a `Car` class that inherits from `Vehicle`
    2.  Define a constructor that takes all of the same arguments as
        `Vehicle`, but that makes `fuel_cap` default to 10, and `speed`
        default to 60. Also add the `seats` argument.
        1.  The number of seats should be stored in `self.seats`
        2.  The rest of the instance variables should be set by calling
            the parent constructor
    3.  Override the `drive(distance)` method to be
        `drive(distance, people)`.
        1.  If the number of people is greater than the number of seats,
            return an error message
        2.  Otherwise, call the parent drive method and return the
            result

Level 4 (Functional Programming)
--------------------------------

### Topics

-   The Functional Abstraction \[15m\]
    -   Immutability and referential transparency
    -   Higher-order functions
    -   Declarative vs imperative
    -   Why use it?
-   Higher-order functions \[15m\]
    -   Built-in (`map`, `filter`, `sorted`, etc)
    -   Lambda functions
-   Iterators \[15m\]
    -   Consuming functions (`sum`, `max`, `zip`, `enumerate`, `all`,
        etc)
    -   Generators (`yield`)
    -   Basic itertools (`islice`, `count`, `cycle`, etc)
-   Generators & Comprehensions \[15m\]
    -   Mapping
    -   Filtering
    -   Generators vs lists

### Exercises

-   Fibonacci Generator
    1.  Write a generator that yields the next Fibonacci number
        infinitely
    2.  Use itertools to get the first N numbers from the generator
-   Triangle Comprehension
    1.  Use a list comprehension to list all possible triangles with
        integer side-lengths of 10 or less (as tuples of the 3 side
        lengths)
    2.  Restrict the comprehension to only list right triangles
        (following Pythagorean theorem and C \>= A + B)
    3.  Find the triangle with a parameter of 24
-   Higher Order Functions
    1.  Reusing the list of right triangles from part 2 of the last
        exercise, extend it to include side-lengths 1-20
    2.  Sort the triangles by largest to smallest parameter
