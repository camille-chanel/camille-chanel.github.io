---
permalink: /lectures102/unix/
title: "Unix"
author_profile: false
classes: wide
---

## The Unix Operating System
An operating system is the underlying program controlling the basic functionality of the computer. Most everyone is probably familiar with the two most popular commerical operating systems in the world - Windows and MacOS (we also use operating systems on our phones - Android and iOS).

In this class, we will learn about another operating system - Linux. Linux is based off of the Unix operating system, an invention from Bell Labs (AT&T precusor). Unix was the first operating system written in a "high level" programming language - the C language (high level compared to assembly language that is). You might hear Unix and Linux used interchangeablely in conversation, but Unix was the original Bell Labs invention, and Linux is the derivation we use today. Linux has continued its popularity today, one reason being it is an open source software (i.e. free to use). The Macinosh operating system (MacOS) is even an underlying Linux sytem.

For history on Unix/Linux, you can read here:
http://ibgwww.colorado.edu/~lessem/psyc5112/usail/concepts/hx-of-unix/unixhx.html
https://www.redhat.com/sysadmin/unix-linux-history

The Hydra and Tesla machines (the computer you use in your lab sections) run on Red Hat, a version of Linux made by Red Hat Inc. (an IBM subsidiary).

### Interfaces
One of the aspects that makes a Linux system compelling is the command line interface. What is the command line interface? First, let's talk about another interface you are more familiar with, so you can understand the differences.

#### Graphical User Interface
With Mac and Windows computers, we interact with the operating systems using graphics. If you want to open a folder that contains your lab files, you use a touchpad or mouse to move a cursor and click on an icon with a folder image. If you want to open Firefox (the internet browser), you click on a fox image. The use of a cursor and images to navigate an operating system is so intuitive at this point you may not think about it anymore, but interacting with a computer in this way is because the operating system is using a *graphical user interface*. Graphics being the key word here.

#### Command Line Interface
While most modern Linux systems do have a graphical user interface that you'll see in your lab, the operating system remains popular due to it's command line interface. Instead of using a cursor and clicking on graphics to navigate your system, you instead type commands on what we call the "command line". You may have seen this in movies, like Tron or Jurassic Park. It's often depicted as a big black box (and often looks like that in real life too).

Clip from the movie Tron: https://youtu.be/9NgGXn682kI

Instead of clicking on a folder called "Documents" to open a file, you would type "cd Documents" to change the directory to Documents.
For example, on a Mac, I would use the Finder as shown below to navigate:

<img src="/images/lectures/finderExample.png" width="800" />

And on a PC, I would use the File Explorer:

<img src="/images/lectures/fileExplorerEx.png" width="800" />

With the command line, again I would just type **cd Documents** if I'm in a folder that contains Programs. (I'll cover what crumpto@CrumptonLaptop:~$ means in the next section).

```console
ccrumpto@CrumptonLaptop:~$ cd Documents
```
You might be asking, how do I get to this place where I can type in commands? That's exactly what we will cover next.

#### How to Access the Command Line Interface
Accessing the command line will be different if you are working on a PC versus a Mac. During the first lab section, the TAs will work with everyone to verify you have your computer setup to access a command line interface.

#### Commands to Know
We will learn a handful of commands in this class. The following are ones you should know for directory navigation and file management:

(table here)

### How We Will Program in CS102

Everything we've covered so far is just about **navigating a new operating system using just the command line**. We haven't talked about coding up a program at all! That's what we will cover next.

In COSC101, you all learned to code in Visual Studio Code - an **IDE**. (If you took an AP class or equivalent pre-req, you probably also learned on an IDE similar to VS Code). An IDE is an **Integrated Developer Environment** which essentially is an application that integrates a lot of tools to help you write a program. Some of the tools you used in CS101 were the run button, the debugger, and the terminal window to view our output.

In this class, we will expand the methods in which you can code. In conjunction with learning Unix, we are going to teach you a text editor called Vim.

#### Vim
Part of CS102 is learning to code without the fancy tools that an IDE like VS Code provides. Vim is a program that we use to code with a command line interface (an interface in which we don't use a cursor/mouse, so we can't "click" on anything). Vim is very simple, but very powerful - and some people learn to code faster when they learn all of Vim's shortcuts. One large benefit these shortcuts and not using a cursor is the ability navigate around a file without lifting you hands from the keyboard. At first this feels bizarre, but the more practice you get with Vim, the more you will be accustomed to it.

We open Vim on the command line when we either want to create a new file or edit an existing file. First, navigate to the directory you want the file to be stored in using **cd [DirName]** where you replace [DirName] with the name of the directory (see Command Line Interface section above for details). I want to make files in my Programs directory.

```console
ccrumpto@CrumptonLaptop:~$ cd Programs
```

Next, I'm going to create a new file called RockyTop.cpp where I print out some verses from everyone's favorite unofficial fight song.

```console
ccrumpto@CrumptonLaptop:~$ vim RockyTop.cpp
```

After you hit enter to execute this command, you have a big blank screen open up. This is your blank file.

<img src="/images/lectures/blankVim.png" width="800" />

