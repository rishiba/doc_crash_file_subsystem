############
Inode Object
############

*   Inode object keeps the information related to the a particular file or a
    directory.

Excecises
=========

For a open file see the following
    
#.  See the details of the inode in the kernel, specifically the following fields.
    
    *   Last modified time
    *   size
    *   file object
    *   number of links
    *   Inode change time.

#.  See the file system specific inode.

#.  See the on disk inode.

#.  Dentry object of the file.

    *   See the file name.

#.  File object of the open file.

#.  File system related inode of the open file.
    
    *   See the number of blocks the file takes.

#.  See the superblock of the open file.

Setup
=====

*	Attach a 100mb file as a loop device.

::

	rishi@lkw:~$ sudo losetup /dev/loop0 100Mb_file

*	Make a file system on the loop device.

::


	rishi@lkw:~$ sudo mkfs.ext4 /dev/loop0 
	mke2fs 1.42.13 (17-May-2015)
	Creating journal (4096 blocks): done

	>>>>> SNIPPED <<<<<<<<<<

	Writing superblocks and filesystem accounting information: done 


*	Mount the file system.

::
	rishi@lkw:~$ sudo mount /dev/loop0 /mnt 
	rishi@lkw:~$ ls /mnt
	lost+found

*	Create a 1mb file on the mounted file system.

::

	rishi@lkw:~$ cd /mnt 
	rishi@lkw:/mnt$ sudo dd if=/dev/zero of=1_mb_file bs=1M count=1
	1+0 records in
	1+0 records out
	1048576 bytes (1.0 MB, 1.0 MiB) copied, 0.00186653 s, 562 MB/s

*	See the file size and other information

::

	rishi@lkw:/mnt$ ls -l
	total 1036
	-rw-r--r-- 1 root root 1048576 Aug 25 07:39 1_mb_file
	drwx------ 2 root root   12288 Aug 25 07:38 lost+found


*	See the ``stat`` of the file. We will use these values to cross check the values obtained from the kernel.

::

	rishi@lkw:/mnt$ stat 1_mb_file 
	  File: '1_mb_file'
	  Size: 1048576   	Blocks: 2048       IO Block: 1024   regular file
	Device: 700h/1792d	Inode: 12          Links: 1
	Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
	Access: 2017-08-25 07:39:27.000000000 -0400
	Modify: 2017-08-25 07:39:27.000000000 -0400
	Change: 2017-08-25 07:39:27.000000000 -0400
	Birth: -

*	Start ``crash``

*	Open the file in ``less``

#.  See the details of the inode in the kernel, specifically the following fields.
    
    *   Last modified time
    *   size
    *   file object
    *   number of links
    *   Inode change time.


*	Get the PID of the file.

::

	crash> ps | grep less
	   7090   6940   0  ffff88000008f000  IN   0.1   12604   4588  less

*	See the open files of the process. Here the first column is the fd, second file object, third the dentry object and fourth inode object. The fifth column shows the type of the file, our file is a ``regular`` file hence ``REG`` is mentioned. Then we have the file name.

::

	crash> files 7090
	PID: 7090   TASK: ffff88000008f000  CPU: 0   COMMAND: "less"
	ROOT: /    CWD: /mnt/
	 FD       FILE            DENTRY           INODE       TYPE PATH
	  0 ffff880034d84e00 ffff880037ae6900 ffff880065562d50 CHR  /dev/pts/1
	  1 ffff880034d84e00 ffff880037ae6900 ffff880065562d50 CHR  /dev/pts/1
	  2 ffff880034d84e00 ffff880037ae6900 ffff880065562d50 CHR  /dev/pts/1
	  3 ffff8800da106200 ffff880116423840 ffff880115c81c88 CHR  /dev/tty
	  4 ffff880034e7dd00 ffff88004eabb3c0 ffff8800d70ebb88 REG  /mnt/1_mb_file

*	Now let us print the inode object.

::

	crash> struct inode ffff8800d70ebb88 
	struct inode {
	  i_mode = 33188, 
	  i_opflags = 5, 
	  i_uid = {
		val = 0
	  }, 
	  i_gid = {
		val = 0
	  }, 
	  i_flags = 4096, 
	  i_acl = 0x0, 

	>>>>>>>>>> SNIPPED <<<<<<<<<<


	  {
		i_pipe = 0x0, 
		i_bdev = 0x0, 
		i_cdev = 0x0, 
		i_link = 0x0
	  }, 
	  i_generation = 1159616597, 
	  i_fsnotify_mask = 0, 
	  i_fsnotify_marks = {
		first = 0x0
	  }, 
	  i_private = 0x0
	}



*	Let us find the size of the file. Cross check it with the value obtained from ``stat``.

::
	crash> struct inode ffff8800d70ebb88 | grep size
  	i_size = 1048576, 

*	Find the last accessed time. You will see a difference wrt to the output of the ``stat`` as the file is accessed a bit later. But modified time will be the same.

::

	crash> struct inode ffff8800d70ebb88 | grep atime -a4
		__i_nlink = 1
	  }, 
	  i_rdev = 0, 
	  i_size = 1048576, 
	  i_atime = {
		tv_sec = 1503662304, <<<<<<<<<<< 
		tv_nsec = 0
	  }, 
	  i_mtime = 


* let us convert the time in epoch to date.

::

 	date -d @1503662304
	Fri Aug 25 07:58:24 EDT 2017


{
crash> struct inode ffff8800d70ebb88 | grep mtime -a4

::
  i_atime = {
    tv_sec = 1503662304, 
    tv_nsec = 0
  }, 
  i_mtime = {
    tv_sec = 1503661167, <<<<<<<<<<<<
    tv_nsec = 0
  }, 
  i_ctime = {

*	Let us convert the epoch into the date.

::

	rishi@lkw:~$ date -d @1503661167
	Fri Aug 25 07:39:27 EDT 2017



	Modify: 2017-08-25 07:39:27.000000000 -0400

*	Let us now see the inode number

::

	crash> struct inode ffff8800d70ebb88 | grep i_ino
	i_ino = 12, 

*	Let us see the number of links.


*	Let us now create some links to the file and see the ``struct inode``.

::
	rishi@lkw:/mnt$ sudo ln 1_mb_file another_link
	[sudo] password for rishi: 
	rishi@lkw:/mnt$ sudo ln 1_mb_file another_link2
	rishi@lkw:/mnt$ sudo ln 1_mb_file another_link3
	rishi@lkw:/mnt$ sudo ln 1_mb_file another_link4
	rishi@lkw:/mnt$ sudo ln 1_mb_file another_link5

*	See the number of links in the ``stat`` command.

	rishi@lkw:/mnt$ stat 1_mb_file 
	  File: '1_mb_file'
	  Size: 1048576   	Blocks: 2048       IO Block: 1024   regular file
	Device: 700h/1792d	Inode: 12          Links: 6
	Access: (0644/-rw-r--r--)  Uid: (    0/    root)   Gid: (    0/    root)
	Access: 2017-08-25 07:58:24.000000000 -0400
	Modify: 2017-08-25 07:39:27.000000000 -0400
	Change: 2017-08-25 08:23:24.000000000 -0400
	 Birth: -

*	As all of them are hardlinks, the inode numbers are same.

::

	rishi@lkw:/mnt$ ls -li 
	total 6156
	12 -rw-r--r-- 6 root root 1048576 Aug 25 07:39 1_mb_file
	12 -rw-r--r-- 6 root root 1048576 Aug 25 07:39 another_link
	12 -rw-r--r-- 6 root root 1048576 Aug 25 07:39 another_link2
	12 -rw-r--r-- 6 root root 1048576 Aug 25 07:39 another_link3
	12 -rw-r--r-- 6 root root 1048576 Aug 25 07:39 another_link4
	12 -rw-r--r-- 6 root root 1048576 Aug 25 07:39 another_link5
	11 drwx------ 2 root root   12288 Aug 25 07:38 lost+found
	rishi@lkw:/mnt$ 


*	Check the number of links in the ``crash`` tool. It is now 6.

::

	crash> struct inode ffff8800d70ebb88 | grep link
		i_nlink = 6, 
		__i_nlink = 6
		i_link = 0x0
	crash> 

#.  See the file system specific inode.

#.  See the on disk inode.

#.  Dentry object of the file.

    *   See the file name.

#.  File object of the open file.

#.  File system related inode of the open file.
    
    *   See the number of blocks the file takes.

#.  See the superblock of the open file.
