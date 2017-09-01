##############
File Subsystem
##############

============
Introduction
============

File Subsystem is responsible for providing the user methods and APIs to
open/read/write/close and other file related operations. All the file handling
related system calls are provided by the file subsystem.

You need to know at least the following operations for a better understanding
of the file subsystem.

This chapter is intended towards giving you a good overview of file subsystem. The chapter will mostly point out to good references and you must read them in order to get a good understanding. Once you go through the references this chapter will show you some of the concepts which you have read in the other references.


===========================
File Subsystem System Calls
===========================

The system calls related to file subsystem are very well documented in the book ``Begining Linux Programming by Neil Matthew and Richard Stones``, chapter number 3 ``Working with Files``. This chapter will give a very good overview of how to use the system calls to get some work done from the kernel.

Amazon: http://www.amazon.in/Beginning-Linux-Programming-Neil-Matthew/dp/0470147628

Small Test
==========

After reading the above reference and doing the exercises you should be able to do the following exercise.

*   Write a program ``mycp`` which gives the functionality of ``cp -u``. Hint: ``stat`` system call.

==================================
File Subsystem Kernel Introduction
==================================

Once you have read the above chapter you now need to go through the second reference which takes you a bit deeper into the file subsystem. You must now read the book ``The design of Unix Operating System`` by ``Maurice J Bach``.

You should go through the chapters 1, 2, 3, 4, and 5. These chapters are very important and give a very good understanding of how things work under the hood.

Amazon: http://www.amazon.in/Design-UNIX-Operating-Maurice-Bach/dp/9332549575/ref=sr_1_1?s=books&ie=UTF8&qid=1502948221&sr=1-1&keywords=the+design+of+the+unix+operating+system

===========================
File Subsystem Linux Kernel
===========================

The VFS layer in the Linux Kernel is the file subsystem in Linux Kernel.  Now you must go through the chapter 12 of the book ``Understanding the Linux Kernel by Daniel P Bovet and Marco Cesati``.

The chapter will explain you in depth all the important concepts in the VFS layer. In the exercises of the book we will be using those concepts to see how the data structures interact with each other in the VFS layer.

