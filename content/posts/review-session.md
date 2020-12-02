---
title: "Review Session"
date: 2020-11-30T22:59:04Z
authors: ["tll"]
categories:
  - Revision
tags:
  - Week 9
  - Challenges
  - Python
  - Arduino
  - Fusion 360
---

Summary
=======

The purpose of this penultimate session is briefly review all that we've covered this term and to apply our newfound knowledge to a variety of programming and design challenges. Several of these challenges will be things you've seen before but might not have had the chance to finish.

Getting Help
============

Even if you've missed the session, you'll be able to ask questions about whatever, whenever on [our Discord](https://discord.gg/N4k7ECt). Feel free to drop your question in the #bootcamp channel or just message an instructor directly.

Python
======

#### The Review

- Week 1: [Basics of Programming & Python](/bionics-general/posts/basics-of-programming-and-python/)
- Week 2: [Modern Python Scripts](/bionics-general/posts/modern-python-scripts/)

#### The New

Check out [list comprehensions](https://realpython.com/list-comprehension-python/#using-list-comprehensions) in Python. These are an incredibly compact way of generating lists (and dictionaries, and sets, and more) without resorting to manually adding elements in a `for` loop.

#### The Challenge For You

Sorry, I couldn't resist the rhyme...

The following problem (like most programming problems) can be solved in a number of ways; with that being said, it does lend itself rather well to list-comprehensions, if you are looking to flex your new knowledge.

Your tasks:

1.  Use a list comprehension (or some other method) to list all possible triangles with integer side-lengths of 10 or less. Each "triangle" should be a tuple containing the length of each side
2.  Filter the list to only contain right triangles (ones that abide by the Pythagorean theorem)
3.  Find the triangle with a parameter of 24

A couple of hints:

- Each of your "triangles" should be a tuple triplet, like this: `(3,4,5)`
- For step 2, you should have a list of either 4 or 2 triangles. If you have 4, take a close look to see if the triangles are really unique, of if two sides have just been swapped
- If you are going the comprehension route, note that you can use several `for ... in`'s in a single comprehension

In the end, the magical triangle you are looking for has side lengths of 6, 8, and 10.

Arduino
=======

#### The Review

- Week 5: [Introduction to Arduino](/bionics-general/posts/introduction-to-arduino/)
- Week 6: [More Arduino](/bionics-general/posts/more-arduino/)

#### The New

Check out [switch statements](https://www.w3schools.com/cpp/cpp_switch.asp) in C++ – these can be used to pick between many code branches using a single value. You may find this a useful tool in assigning functionality to your keypad.

Additionally, while many problems in computing can be solved rather intuitively (something Python excels at), sometimes lower-level work (like microcontroller programming) can benefit from numerical tricks. Without using Strings or arrays, think about how you could add a digit to a number (e.g. adding the digit 4 to 123 would result in 1234).

#### The Challenge For You

This is a callback to Week 6 and an opportunity to finish what we started working on back then (if you haven't already).

Using the [partially complete pin-pad](https://www.tinkercad.com/things/fKLWtGpt8nH) from the end of the session, implement a circuit and program that:

1. Can detect single button presses on the keypad
2. Can keep track of a multi-digit number input from the keypad
3. Sets the servo position equal to the number of degrees specified by the keypad input

Give this a proper go and feel free to use the recording and / or any of the materials linked below. If you really get stuck, or would like to compare your solution to mine, the ["solution" is here](https://www.tinkercad.com/things/8RGvzMR2Qva).

Fusion 360
==========

#### The Review

- Week 7: [CAD in Fusion 360](/bionics-general/posts/cad-in-fusion-360/)
- Week 8: [More CAD in Fusion 360](/bionics-general/posts/more-cad-in-fusion-360/)

#### The New

Fusion 360 doesn't just allow you to design individual parts, it also allows you to assemble them into more complex structures. Check out [this video](https://www.youtube.com/watch?v=t41QmQszcbEcx) for an introduction to part assembly if you're curious. You can follow along with the video by downloading [the same set of parts here](https://a360.co/34eTv4K).

#### The Challenge For You

To practice your part design skills, try replicating the part shown below. Assume that the centre of the hole on the top is placed 30mm from the front face (unless you can find evidence to the contrary).

![A image of the 3D part to replicate](/bionics-general/images/FusionChallenge.png)

To check your answer, see [the model here](https://a360.co/2oDDxzf). When made out of solid Aluminium 6061, the part should weigh 388.415g.

Recording
=========

The session hasn't happened just yet, but check back for the full recording later!

Feedback
========

Don't forget to fill in [this week's survey](https://forms.gle/N7Fqsi4xX65ET5Fv6)!