###########
File Object
###########

*   A File object keeps the information related to a file opened by a process.
    It points to the its inode, but here the difference is that the file obejct is
    always in context of the process which has opened the file. You can have
    several file objects pointing to the same inode. For every ``open`` call made
    to same file you will get a new file object but you wont get a new inode object
    for every ``open`` call.


Exercises
=========

For an open file see the following

#.  See the current file offset. We will open the file and read 1K of the file. This will place the file pointer to 1K. Check this value from the Crash.

#.  See the data written on the file. The data will be in some pages, go to the pages and read the data.
