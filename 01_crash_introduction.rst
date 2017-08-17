##########
Crash Tool
##########

============
Introduction
============


*   ``Crash`` is a tool which is widely used for Linux Kernel debugging.
*   It allows users to debug the crashed Linux Kernels as well as Live Running Kernels.
*   In this section, we will see how to install and run the crash tool on a Ubuntu Server 16.04 LTS.
*   Crash tool will serve as the basis of the whole book's exercises. Using this tool, we will be seeing all the data structures in the system.


=====================
Setting Up Crash Tool
=====================

*   Installing Crash tool.

::

    $ sudo apt-get install linux-crashdump
    [sudo] password for rishi: 
    Reading package lists... Done
    Building dependency tree       
    Reading state information... Done
    The following additional packages will be installed:
    crash kdump-tools kexec-tools libdw1 makedumpfile
    The following NEW packages will be installed:
    crash kdump-tools kexec-tools libdw1 linux-crashdump makedumpfile
    0 upgraded, 6 newly installed, 0 to remove and 456 not upgraded.
    Need to get 2,964 kB of archives.
    After this operation, 9,395 kB of additional disk space will be used.
    Do you want to continue? [Y/n] y
    Get:1 http://in.archive.ubuntu.com/ubuntu xenial-updates/main amd64 crash amd64 7.1.4-1ubuntu4.1 [2,529 kB]
    Get:2 http://in.archive.ubuntu.com/ubuntu xenial/main amd64 libdw1 amd64 0.165-3ubuntu1 [191 kB]
    Get:3 http://in.archive.ubuntu.com/ubuntu xenial-updates/main amd64 makedumpfile amd64 1:1.5.9-5ubuntu0.3 [145 kB]
    Get:4 http://in.archive.ubuntu.com/ubuntu xenial/main amd64 kexec-tools amd64 1:2.0.10-1ubuntu2 [77.3 kB]
    Get:5 http://in.archive.ubuntu.com/ubuntu xenial-updates/main amd64 kdump-tools all 1:1.5.9-5ubuntu0.3 [20.6 kB]
    Get:6 http://in.archive.ubuntu.com/ubuntu xenial-updates/main amd64 linux-crashdump amd64 4.4.0.62.65 [2,514 B]
    Fetched 2,964 kB in 3s (898 kB/s)     
    Preconfiguring packages ...
    Selecting previously unselected package crash.
    (Reading database ... 178576 files and directories currently installed.)
    Preparing to unpack .../crash_7.1.4-1ubuntu4.1_amd64.deb ...
    Unpacking crash (7.1.4-1ubuntu4.1) ...
    Selecting previously unselected package libdw1:amd64.
    Preparing to unpack .../libdw1_0.165-3ubuntu1_amd64.deb ...
    Unpacking libdw1:amd64 (0.165-3ubuntu1) ...
    Selecting previously unselected package makedumpfile.
    Preparing to unpack .../makedumpfile_1%3a1.5.9-5ubuntu0.3_amd64.deb ...
    Unpacking makedumpfile (1:1.5.9-5ubuntu0.3) ...
    Selecting previously unselected package kexec-tools.
    Preparing to unpack .../kexec-tools_1%3a2.0.10-1ubuntu2_amd64.deb ...
    Unpacking kexec-tools (1:2.0.10-1ubuntu2) ...
    Selecting previously unselected package kdump-tools.
    Preparing to unpack .../kdump-tools_1%3a1.5.9-5ubuntu0.3_all.deb ...
    Unpacking kdump-tools (1:1.5.9-5ubuntu0.3) ...
    Selecting previously unselected package linux-crashdump.
    Preparing to unpack .../linux-crashdump_4.4.0.62.65_amd64.deb ...
    Unpacking linux-crashdump (4.4.0.62.65) ...
    Processing triggers for man-db (2.7.5-1) ...
    Processing triggers for libc-bin (2.23-0ubuntu3) ...
    Processing triggers for ureadahead (0.100.0-19) ...
    Processing triggers for systemd (229-4ubuntu4) ...
    Setting up crash (7.1.4-1ubuntu4.1) ...
    Setting up libdw1:amd64 (0.165-3ubuntu1) ...
    Setting up makedumpfile (1:1.5.9-5ubuntu0.3) ...
    Setting up kexec-tools (1:2.0.10-1ubuntu2) ...
    Generating /etc/default/kexec...
    Generating grub configuration file ...
    Warning: Setting GRUB_TIMEOUT to a non-zero value when GRUB_HIDDEN_TIMEOUT is set is no longer supported.
    Found linux image: /boot/vmlinuz-4.4.0-21-generic
    Found initrd image: /boot/initrd.img-4.4.0-21-generic
    Found memtest86+ image: /boot/memtest86+.elf
    Found memtest86+ image: /boot/memtest86+.bin
    done
    Setting up kdump-tools (1:1.5.9-5ubuntu0.3) ...
    Setting up linux-crashdump (4.4.0.62.65) ...
    Processing triggers for libc-bin (2.23-0ubuntu3) ...
    Processing triggers for ureadahead (0.100.0-19) ...
    Processing triggers for systemd (229-4ubuntu4) ...


*   A window comes which ask you for permission. Give it ``yes``

*   Verify 

::

    grep USE_KDUMP  /etc/default/kdump-tools 
    # USE_KDUMP - controls kdump will be configured
    USE_KDUMP=1


*   Install the debug symbols for the kernel. We will check the kernel version first and then we will find the related debug symbol deb package.

::

    $ uname -a 
    Linux lkw 4.4.0-62-generic #83-Ubuntu SMP Wed Jan 18 14:10:15 UTC 2017 x86_64 x86_64 x86_64 GNU/Linux

*   Our kernel version is 4.4.0-62 #83. We can get the symbol deb package over here https://launchpad.net/ubuntu/xenial/amd64/linux-image-4.4.0-62-generic-dbgsym/4.4.0-62.83




*   Download the debug package and install it.

::

	wget http://launchpadlibrarian.net/302967523/linux-image-4.4.0-62-generic-dbgsym_4.4.0-62.83_amd64.ddeb 
	--2017-08-16 07:35:25--  http://launchpadlibrarian.net/302967523/linux-image-4.4.0-62-generic-dbgsym_4.4.0-62.83_amd64.ddeb
	Resolving launchpadlibrarian.net (launchpadlibrarian.net)... 91.189.89.229, 91.189.89.228
	Connecting to launchpadlibrarian.net (launchpadlibrarian.net)|91.189.89.229|:80... connected.
	HTTP request sent, awaiting response... 200 OK

*   Install the package. 

::

    $ sudo dpkg -i linux-image-4.4.0-62-generic-dbgsym_4.4.0-62.83_amd64.ddeb 
    [sudo] password for rishi: 
    Selecting previously unselected package linux-image-4.4.0-62-generic-dbgsym.
    (Reading database ... 67826 files and directories currently installed.)
    Preparing to unpack linux-image-4.4.0-62-generic-dbgsym_4.4.0-62.83_amd64.ddeb ...
    Unpacking linux-image-4.4.0-62-generic-dbgsym (4.4.0-62.83) ...
	Setting up linux-image-4.4.0-62-generic-dbgsym (4.4.0-62.83) ...


*   Start crash, we will have to give the location on the debug vmlinux file in the command line.

::

	sudo crash /usr/lib/debug/boot/vmlinux-4.4.0-62-generic 

	crash 7.1.4
	Copyright (C) 2002-2015  Red Hat, Inc.
	Copyright (C) 2004, 2005, 2006, 2010  IBM Corporation
	Copyright (C) 1999-2006  Hewlett-Packard Co
	Copyright (C) 2005, 2006, 2011, 2012  Fujitsu Limited
	Copyright (C) 2006, 2007  VA Linux Systems Japan K.K.
	Copyright (C) 2005, 2011  NEC Corporation
	Copyright (C) 1999, 2002, 2007  Silicon Graphics, Inc.
	Copyright (C) 1999, 2000, 2001, 2002  Mission Critical Linux, Inc.
	This program is free software, covered by the GNU General Public License,
	and you are welcome to change it and/or distribute copies of it under
	certain conditions.  Enter "help copying" to see the conditions.
	This program has absolutely no warranty.  Enter "help warranty" for details.
	 
	GNU gdb (GDB) 7.6
	Copyright (C) 2013 Free Software Foundation, Inc.
	License GPLv3+: GNU GPL version 3 or later <http://gnu.org/licenses/gpl.html>
	This is free software: you are free to change and redistribute it.
	There is NO WARRANTY, to the extent permitted by law.  Type "show copying"
	and "show warranty" for details.
	This GDB was configured as "x86_64-unknown-linux-gnu"...

	crash: read error: kernel virtual address: ffffffff81a11e30  type: "cpu_possible_mask"
	crash: this kernel may be configured with CONFIG_STRICT_DEVMEM, which
		   renders /dev/mem unusable as a live memory source.
	crash: trying /proc/kcore as an alternative to /dev/mem

		  KERNEL: /usr/lib/debug/boot/vmlinux-4.4.0-62-generic
		DUMPFILE: /proc/kcore
			CPUS: 2
			DATE: Wed Aug 16 08:30:56 2017
		  UPTIME: 01:22:14
	LOAD AVERAGE: 0.66, 0.33, 0.13
		   TASKS: 178
		NODENAME: lkw
		 RELEASE: 4.4.0-62-generic
		 VERSION: #83-Ubuntu SMP Wed Jan 18 14:10:15 UTC 2017
		 MACHINE: x86_64  (2394 Mhz)
		  MEMORY: 3.9 GB
			 PID: 2469
		 COMMAND: "crash"
			TASK: ffff8800db8ac600  [THREAD_INFO: ffff8800d5f14000]
			 CPU: 1
		   STATE: TASK_RUNNING (ACTIVE)

	crash> ps
	   PID    PPID  CPU       TASK        ST  %MEM     VSZ    RSS  COMM
		  0      0   0  ffffffff81e11500  RU   0.0       0      0  [swapper/0]
	>     0      0   1  ffff8801163e8e00  RU   0.0       0      0  [swapper/1]
		  1      0   1  ffff880116398000  IN   0.1   37664   5864  systemd
		  2      0   1  ffff880116398e00  IN   0.0       0      0  [kthreadd]
		  3      2   0  ffff880116399c00  IN   0.0       0      0  [ksoftirqd/0]
		  5      2   0  ffff88011639b800  IN   0.0       0      0  [kworker/0:0H]
		  7      2   0  ffff88011639d400  IN   0.0       0      0  [rcu_sched]
		  8      2   0  ffff88011639e200  IN   0.0       0      0  [rcu_bh]
		  9      2   0  ffff88011639f000  IN   0.0       0      0  [migration/0]
		 10      2   0  ffff8801163e8000  IN   0.0       0      0  [watchdog/0]
		 11      2   1  ffff8801163e9c00  IN   0.0       0      0  [watchdog/1]


==============
Crash Commands
==============

We have seen some of the data structures in the Linux Kernel which is used by the process or the file subsystem.

``Crash`` tool has a lot of commands using which you can see the live data structures of the kernel.

In this chapter we will see those commands.

.. todo:: write this section.

Reference
=========

http://blog.zedroot.org/linux-kernel-debuging-using-kdump-and-crash/
http://blog.zedroot.org/linux-kernel-debuging-using-kdump-and-crash/

https://unix.stackexchange.com/questions/308889/gpg-keyserver-receive-failed-keyserver-error

https://wiki.ubuntu.com/Debug%20Symbol%20Packages


https://launchpad.net/ubuntu/xenial/amd64/linux-image-4.4.0-62-generic-dbgsym/4.4.0-62.83
