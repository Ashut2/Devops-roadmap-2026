# Day 2 – Linux Internals & User Management

## Linux filesystem structure
- /etc → configuration files
- /var → variable data (logs, cache)
- /bin → essential user binaries
- /sbin → system/admin binaries

## Command execution
- Commands like `ls` work because of $PATH
- Example: /usr/bin/ls

## User management
- adduser → interactive, user-friendly
- useradd → low-level command
- Switch user: su - <username>
- Root access via sudo su

## Security notes
- Passwords are stored in encrypted form
- /etc/shadow contains password hashes
- Passwords cannot be decrypted, only reset

## Commands practiced
- adduser / useradd
- userdel
- su
- sudo su
- echo $PATH

