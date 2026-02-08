# Day 1 – Linux Basics

## What I learned
- What Linux is
  linux is a OS which let actors like user and application to talk with haardware , Kernel is the most important component of the linux based system.
- What terminal and shell are
  they are CLI through which user can communicate with the hardware
- Basic file system structure
  /
├── bin/     (programs)
├── boot/    (boot files)
├── dev/     (devices)
├── etc/     (config files)
├── home/    (user folders)
│   └── ashu/
├── root/    (root's home)
├── tmp/     (temporary)
├── usr/     (user programs)
├── var/     (variable data/logs)
└── mnt/     (mount points)

the basic Linux file system structure:
Root Directory (/)
Everything in Linux starts from the root directory /. All files and directories branch out from here.
Key Directories:
/home - User home directories

/home/ashu - Your personal files and settings
Each user gets their own folder here

/root - Root user's home directory

Separate from /home
Only accessible by the superuser

/bin - Essential user binaries (programs)

Basic commands like ls, cp, cat, echo
Available to all users

/sbin - System binaries

System administration commands like reboot, fdisk
Usually require root privileges

/etc - Configuration files

System-wide configuration files
Example: /etc/passwd (user info), /etc/hosts (network)

/var - Variable data

/var/log - Log files
/var/tmp - Temporary files that persist across reboots

/tmp - Temporary files

Cleared on reboot
Anyone can write here

/usr - User programs and data

/usr/bin - Non-essential user programs
/usr/lib - Libraries for programs
/usr/local - Locally installed software

/opt - Optional software packages

Third-party applications

/dev - Device files

Hardware devices represented as files
Example: /dev/sda (hard drive)

/proc - Process information

Virtual filesystem with system/process info
Example: /proc/cpuinfo

/mnt - Mount point for temporary mounts

In WSL: /mnt/c is your Windows C: drive

/media - Mount point for removable media

USB drives, CD-ROMs

/boot - Boot loader files

Kernel and files needed to boot

/lib - Essential shared libraries

Libraries needed by binaries in /bin and /sbin

## Commands practiced
- pwd : gives current directory user in .
- ls
- cd
- mkdir
- touch
- cp
- mv
- rm
- cat
- uname : gives the name of your distribution system
- head
- tail
- man <cmd_name> : gives manual of that command 

## Notes
- Linux is case-sensitive
- Everything is treated as a file
- linux was brought into picture on 1990
- created an apache user name jim and give him the uniue id 1738 at /var/www/jim
  /connect to app server 2
  ssh steve@stapp02
  /create user
  sudo useradd -u 1738 -d /var/www/jim -m jim
  u - uid
  -d - directory
  -m = create a home directory if doesn't exist
  /now you can verify the user
  id jim
## Extra learning
you can create folders in github repo by just typing 
/week1/day1
and you will have a structure like
/week 1
   |
   | /day1 
