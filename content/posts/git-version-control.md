---
title: "Git Version Control"
date: 2020-10-21T15:26:24+01:00
authors: ["tll"]
categories:
  - Software
tags:
  - Week 3
  - Level 1
  - Git
  - Version Control
---

Getting Started
===============

Installing Git
--------------

To work with Git, we\'ll need it installed! It works on Mac, Windows,
and Linux and is a very small download. See
[here](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)
for installation instructions.

Signing up for GitLab
---------------------

Additionally, please create a GitLab account using [this
link](https://gitlab.com/users/sign_in#register-pane). This will allow
you to share your code and designs with Bionics!

Joining the Bionics Organisation
--------------------------------

Finally, please send us your GitLab username using [this
form](https://forms.gle/veijM3qMT8WKdV3j8) so that we can add you to the
Bionics organisation!

Introduction
============

What *is* Git?
--------------

Git is the most popular Version Control Software (or VCS) used today.
Broadly speaking, VCS enables you to keep a version history of the files
on your computer.

What can I do?
--------------

In addition to just keeping a history, Git allows for parallel
histories. This feature, called branching, enables you to save all of
your work; create a parallel history; create, update, and destroy
whatever you please; then return back to your untouched version at a
moment\'s notice.

Finally, Git repositories can be pushed to websites called \"remotes\".
There are many of these and GitLab, GitHub, and Bitbucket are just a
couple of examples. Once pushed to a remote, all of your files *and*
their histories are safely backed up and can be shared with others.

Why should I care?
------------------

The distributed nature of Git and the ease of branching makes it trivial
for different people to work on a project at the same time without
stepping on each other\'s toes. Countless open-source projects use Git
as their primary tool for collaboration (like the volunteer-driven
[Exercism](https://github.com/exercism/) project).

Commands to know
================

Below is a small subset of Git commands that I find the most important
to know. If you\'d like something a bit more comprehensive, please check
out [this
cheat-sheet](https://about.gitlab.com/images/press/git-cheat-sheet.pdf)
by GitLab.

First Time Setup
----------------

`git config --global user.name`
:   Set the name that appears in your Git commits

`git config --global user.email`
:   Set the email that appears in your Git commits

Fetching or Creating a Repository
---------------------------------

`git init`
:   Create a new Git repository on your computer

`git clone`
:   Create a local version of a remote repository

`git pull`
:   Update a repository you\'ve already cloned

Poking Around
-------------

`git status`
:   Do I have uncommitted work? Is the remote up to date?

`git diff`
:   What\'s changed since my last commit?

`git log`
:   What previous commits have been made?

`ls`
:   Not actually a Git command, can list files in the current directory

`cd`
:   Not actually a Git command, can be used to change the current
    directory

Adding and Committing
---------------------

`git add (-A)`
:   Add a file or folder to the staging area; add all files with `-A`

`git commit -m`
:   Commit the work in the staging area with the message given after
    `-m`

Branching
---------

`git branch`
:   Create a new branch

`git checkout (-b)`
:   Move to a particular branch; create a new one if it doesn\'t exist
    with `-b`

`git merge`
:   Rejoin two branches, combining the histories

Sharing Your Work
-----------------

`git push`
:   Publish your current branch to the remote

Challenge
=========

1.  Pull the `say-hello` repository from GitLab
    [here](https://gitlab.com/sheffield-bionics/say-hello)
2.  Create a new Git branch and switch to it
3.  Create a new Python (`.py`) file in the `greetings` folder, then add
    a function or two. This could just be a \"Hello, World!\", or you
    could share some of your solutions to the challenge problems from
    weeks 1 and 2!
4.  Edit the `say-hello.py` file to import your new Python file. If your
    file was called `brooks.py`, then something like
    `import greetings.brooks as
      brooks` would suffice.
5.  Later in `say-hello.py`, add a line or two calling the functions you
    defined in your greetings file (e.g. `brooks.howdy()`)
6.  Stage and commit your changes to your branch
7.  Push your new branch to GitLab
8.  Open a Merge Request (MR) for your branch on GitLab

Useful Links
============

-   [Visual Git Tutorial](https://learngitbranching.js.org/)
-   [The Free, Official Git Book](https://git-scm.com/book/en/v2)

Recording
=========

Sorry for the god-awful audio, I've since bought a new microphone and am working on growing a couple of brain-cells.

{{< youtube Jxl2tZvUBZ0 >}}

Feedback
========

Don't forget to fill in [this week's survey](https://forms.gle/r1mGLR2YKGHw9upW9)!