## Log
- created file nodehealth.sh
- learnt best practices to follow while writing bash scripts.
- difference between grep and awk
- Interview question: Output of : Date | echo "Today is"


## nodehealth.sh content
---
#!/bin/bash

//comment

Author: Ashutosh
 Date: 03-04-2026

 This scripts prints the node health.

 Version: v1

//comment


// use set to set the shell script to debug mode; It prints the command before executing it.


set -x #debug mode

df -h

free -g

nproc

ps -ef

---

# awk
- grep gives us the whole thing whereas awk has more control over output and can output specific columns.

- syntax: `ps -ed | grep amazon | awk -F" " '{print $4}'` = Prints the 4th column of the process lsit table

# Date question 

Q ) Output of the command  `Date | echo "today is "` ?

Ans : today is 

`Reason` : Date is a system defaut command which unlike any command sends the output to stdin and hence pipe operator didn't get anything as input to 
pass on to the next command. 

## Live display 

---
ubuntu@ip-id:~/bash_scripting$ date

Thu Apr  2 20:21:39 UTC 2026

ubuntu@ip-id:~/bash_scripting$ date | echo "Today is"

Today is

ubuntu@ip-id:~/bash_scripting$

---
