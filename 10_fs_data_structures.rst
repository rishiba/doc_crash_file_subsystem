##############################
File Subsystem Data Structures
##############################

For understanding the above - you will have to understand some of the data
structures which are used in the file subsystem to maintain the required data.

============
Introduction
============

BY now you would have understood about the data structures in the VFS Layer. These the important data structres.


vfsmount
========

*   Stucture Name : struct vfsmount
*   Purpose       : keep the list of the mounted superblocks.

Superblock
==========

*   Structure Name  : struct super_block
*   Purpose         : keep the information related to a mounted file system.  

Dentry
======

*   Structure Name  :   struct dentry
*   Purpose         : keep the information of a directory entry.

Inode
=====


*   Structure Name  : struct inode
*   Purpose         : keep the information of the inode of a open file. 

File
====

*   Structure Name  : struct file
*   Purpose         : keep the information of an instance of an open file.

=====
Setup
=====

*   We will now setuup our system in order to make it usable for further exercises.

Let us make a ext4 file system for our test usage.

*   Make a file with name as ``ext4``. The name can be anything.

::

    rishi@lkw:~$ dd if=/dev/zero of=ext44 bs=1M count=110
    110+0 records in
    110+0 records out
    115343360 bytes (115 MB, 110 MiB) copied, 0.0637954 s, 1.8 GB/s
    rishi@lkw:~$ dd if=/dev/zero of=ext44 bs=1M count=100
    100+0 records in
    100+0 records out
    104857600 bytes (105 MB, 100 MiB) copied, 0.0786865 s, 1.3 GB/s
    rishi@lkw:~$ 


*   attach a loop device to the file so that the file can be used as a block device.

::

    rishi@lkw:~$ sudo losetup /dev/loop0 ext4 

*   Make a ext4 file system on the loop device

::

    rishi@lkw:~$ sudo mkfs.ext4 /dev/loop0 
    mke2fs 1.42.13 (17-May-2015)
    /dev/loop0 contains a ext4 file system
    created on Thu Aug 17 05:51:50 2017
    Proceed anyway? (y,n) y
    Discarding device blocks: done                            
    Creating filesystem with 102400 1k blocks and 25688 inodes
    Filesystem UUID: e368ab45-22f9-4384-ac3c-8d5def6c4af0
    Superblock backups stored on blocks: 
    8193, 24577, 40961, 57345, 73729

    Allocating group tables: done                            
    Writing inode tables: done                            
    Creating journal (4096 blocks): done
    Writing superblocks and filesystem accounting information: done 

*   Mount the loop device.

::
    rishi@lkw:~$ sudo mount /dev/loop0 /mnt 

*   See the mounted location.

::

    $ ls /mnt
    lost+found


==========
Conclusion
==========

*   In this chapter we have seen some information about the data structures in the VFS layer.
*   Also - we have setup the file system required to go forward with the assignments in the book.
