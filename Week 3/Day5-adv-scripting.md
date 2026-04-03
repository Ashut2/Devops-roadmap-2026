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

## awk
- grep gives us the whole thing whereas awk has more control over output and can output specific columns.

- syntax: `ps -ed | grep amazon | awk -F" " '{print $4}'` = Prints the 4th column of the process lsit table

## Date question 

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

## set -exo pipeline

- It is the standard practice used during scripting.

`set -e` : //it is used to add a error callout feature; if script has wrong command in 1st line and rest is correct , without `set -e` the script would run perfectly.

`set -o pipefail` : // while executing the script , linux looks at the last command in the pipe if it is correct then the script is executed smoothly but with this command in use , any error in command would result in failing to execute (./file_name) the file.  

## finding errors in a logfile

1 ) `curl` : this directly fetches the content of the file from the internet without downloading it in the local storage.

2) `wget` : Downloads the logfile -> use grep command on the downloaded file to find errors

### commands : 

`curl  url_of_the_logfile | grep error` 

`wget file_url` -> `cat file_name | grep error`

## find

- It is used to find specific file in the system or vm.
- like finding a pem file ? : `find / -name file_name`
- you have to elevate the user to the root for this command.

## conditional and loop statements 

### If else
if [Expression]
then 
     statement 1 (probably be the echo command to print the text or message.)
     statement 2
else
      statement 3
fi // to close the if block in scipts.

### for loop

`for i in {1..100}; do echo $1; done`

- `for i in {1..100};` : This initializes a loop that will run 100 times, using the sequence of numbers from 1 to 100.

- `do echo $1;` : This is the action performed in each iteration. It prints the first command-line argument (represented by $1) passed to the script.

- `done`: This signals the end of the loop block.


If you run the version with a single dot ({1.100}), Bash may treat it as a literal string rather than a numerical range, and the loop might only run once.

- What it does in practice
  - If you saved this in a script called `test.sh` and ran `./test.sh Hello` , the terminal would print the word "Hello" 100 times.
