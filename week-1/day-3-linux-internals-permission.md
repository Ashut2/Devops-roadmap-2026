# Week 1 – Day 2 (Linux Internals + Permissions)

## Linux filesystem structure
- /etc → system and application configuration files
- /var → variable data like logs, cache, spool
- /bin → essential user commands
- /sbin → system administration commands

Understanding the filesystem helped me see Linux as a system, not just commands.

## Command execution and PATH
- Commands like `ls` work because they exist in directories listed in $PATH
- Example: /usr/bin/ls
- PATH decides how the shell finds executables

## User and root management
- adduser → interactive, high-level command
- useradd → low-level, manual command
- su - <username> → switch user with full environment
- sudo su → temporary root access

## Password and security basics
- User passwords are stored as hashes
- Passwords cannot be decrypted once forgotten
- /etc/shadow contains encrypted password hashes
- Passwords can only be reset, not recovered

## File permissions
- r = read, w = write, x = execute
- Permissions apply to owner, group, others
- 777 → full access (unsafe)
- 644 → default safe file permission
- 600 → private/secret files

## Ownership
- Every file has an owner and a group
- chown changes ownership
- Permissions without correct ownership are ineffective

## Why sudo exists
- Root has unrestricted power
- sudo provides controlled, temporary privilege escalation
- Follows the principle of least privilege
- Reduces accidental system damage

## Commands practiced
- ls -l
- chmod
- chown
- whoami
- groups
- adduser / useradd
- userdel
- su
- sudo su
- echo $PATH
