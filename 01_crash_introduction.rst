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


*   Verify 

::

    grep USE_KDUMP  /etc/default/kdump-tools 
    # USE_KDUMP - controls kdump will be configured
    USE_KDUMP=1


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
