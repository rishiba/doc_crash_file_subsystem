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

#.  Get the file object.

#.  See the current file offset. We will open the file and read 1K of the file. This will place the file pointer to 1K. Check this value from the Crash.

#.  See the data written on the file. The data will be in some pages, go to the pages and read the data.


.. _get_file_object:

Get the file object.
====================

*   ``files`` command takes the process Id and prints the ``file objects`` of the open files.

*   In the following output the first column is of the of the ``file object``

::
    crash> files 10133
    PID: 10133  TASK: ffff88000008aa00  CPU: 1   COMMAND: "less"
    ROOT: /    CWD: /mnt/
    FD       FILE            DENTRY           INODE       TYPE PATH
    0 ffff8800d881e200 ffff880056a193c0 ffff880056b9daa0 CHR  /dev/pts/2
    1 ffff8800d881e200 ffff880056a193c0 ffff880056b9daa0 CHR  /dev/pts/2
    2 ffff8800d881e200 ffff880056a193c0 ffff880056b9daa0 CHR  /dev/pts/2
    3 ffff880115057000 ffff880116423840 ffff880115c81c88 CHR  /dev/tty
    4 ffff8800dad9e800 ffff88004eabb3c0 ffff8800d70ebb88 REG  /mnt/1_mb_file

#.  See the current file offset. We will open the file and read 1K of the file. This will place the file pointer to 1K. Check this value from the Crash.

#.  See the data written on the file. The data will be in some pages, go to the pages and read the data.
