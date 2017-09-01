#############
Dentry Object
#############

*   Dentry object keeps information related to a opened or in use folder. It
    also points to its inode.


Exercises
=========


For an open file see the following

#.  Get the dentry of open files.

#.  See the parent directory of the open file.

#.  Find the full path of a file.


.. _get_dentry_object:

Dentry object of the file.
==========================

::

    PID: 10133  TASK: ffff88000008aa00  CPU: 1   COMMAND: "less"
    ROOT: /    CWD: /mnt/
     FD       FILE            DENTRY           INODE       TYPE PATH
      0 ffff8800d881e200 ffff880056a193c0 ffff880056b9daa0 CHR  /dev/pts/2
      1 ffff8800d881e200 ffff880056a193c0 ffff880056b9daa0 CHR  /dev/pts/2
      2 ffff8800d881e200 ffff880056a193c0 ffff880056b9daa0 CHR  /dev/pts/2
      3 ffff880115057000 ffff880116423840 ffff880115c81c88 CHR  /dev/tty
      4 ffff8800dad9e800 ffff88004eabb3c0 ffff8800d70ebb88 REG  /mnt/1_mb_file

*	In the last line you have the dentry object of the file. Let us print it.

::

	crash> struct dentry ffff88004eabb3c0
	struct dentry {
	  d_flags = 4718784, 
	  d_seq = {
		sequence = 4
	  }, 
	  d_hash = {
		next = 0x0, 
		pprev = 0xffffc90000120b28
	  }, 
	  d_parent = 0xffff88003782fa80, 
	  d_name = {
		{
		  {
			hash = 845259025, 

	>>>>>>>>>>> SNIPPED <<<<<<<<<<<<<<<<



See the parent directory of the open file.
==========================================

*   The parent dentry is stored in the field ``d_parent`` of the ``dentry``. Let us print that.

*   Let us make a directory with 3 levels and create a file in it.


::
    
    root@lkw:/mnt# mkdir -p level1/level2/level3
    root@lkw:/mnt# touch level1/level2/level3/small_file

*   The directory structure should look like this.

::

    # tree level1/
    level1/
    └── level2
        └── level3
                └── small_file


*   Open the file in ``less`` utility.

*	See the open files of the process


::

	crash> ps | grep less
	  11266  10893   1  ffff8800d9fcf000  IN   0.1    8380   2348  less
	crash> files 11266
	PID: 11266  TASK: ffff8800d9fcf000  CPU: 1   COMMAND: "less"
	ROOT: /    CWD: /mnt/
	 FD       FILE            DENTRY           INODE       TYPE PATH
	  0 ffff8800dadd0100 ffff880056845c00 ffff88006559bcd8 CHR  /dev/pts/1
	  1 ffff8800dadd0100 ffff880056845c00 ffff88006559bcd8 CHR  /dev/pts/1
	  2 ffff8800dadd0100 ffff880056845c00 ffff88006559bcd8 CHR  /dev/pts/1
	  3 ffff8800da73c200 ffff880116423840 ffff880115c81c88 CHR  /dev/tty
	  4 ffff8800da00b100 ffff88005d9750c0 ffff8801166d6ef8 REG  /mnt/level1/level2/level3/small_file

*	See the name of the dentry object.

::

	crash> struct dentry ffff88005d9750c0 | grep " name =" 
		name = 0xffff88005d9750f8 "small_file"

*	Get the parent dentry of the dentry.

::

	crash> struct dentry ffff88005d9750c0 | grep " d_parent"
	  d_parent = 0xffff88005d975540, 
	crash> 

*	See the name of the parent dentry.

::
    
	crash> struct dentry 0xffff88005d975540, | grep " name ="
    	name = 0xffff88005d975578 "level3"


Get the full path of an opened file
====================================

*   We will walk through the parent dentries and find the dentry with name as ``/``. This will give us the full path.

*	See the parent dentry of the object.

::

	crash> struct dentry 0xffff88005d975540, | grep " d_parent"
	  d_parent = 0xffff88005d975b40, 

*	Do the same process unless the name is ``/``. This way you can get the full path of a opened file. 

::

	crash> struct dentry 0xffff88005d975b40, | grep " name = "
		name = 0xffff88005d975b78 "level2"
	crash> struct dentry 0xffff88005d975b40 | grep " d_parent"
	  d_parent = 0xffff8800db15bd80, 
	crash> struct dentry 0xffff8800db15bd80 | grep " name = "
		name = 0xffff8800db15bdb8 "level1"
	crash> struct dentry 0xffff8800db15bd80 | grep " d_parent"
	  d_parent = 0xffff88003782fa80, 
	crash> struct dentry 0xffff88003782fa80, | grep " name = "
		name = 0xffff88003782fab8 "/"
	crash> struct dentry 0xffff88003782fa80, | grep " d_parent"
	  d_parent = 0xffff88003782fa80, 

*   So the full path is ``/level1/level2/level3/small_file``. This is the path of the mounted file system.
