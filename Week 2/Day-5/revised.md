## Two Things I revised 
 ### How to change Permission for ssh root login 
  - refer to day-3 repo for complete problem and solution : https://github.com/Ashut2/Devops-roadmap-2026/blob/main/Week%202/Day-3/day3-lab.md
 ### studied Nautilus architecture a liitle bit from  documentation 
  - Leanred about acronym LAMP : Linux Os , Apache Http Server , Mysql relational database, PHP.
  - refer to document for detailed information : https://kodekloudhub.github.io/kodekloud-engineer/docs/projects/nautilus#
---

## Question 
  ### Why disabling root ssh matters?
   - Hackers need two information to hack into our ssh servers : Username & password
   - Username for the root user in almost every linux system is the same : root
   - The only thing left is to guess out the password - which increases the concern of infrastructure security.
   - Hence we need an extra layer of security .
   - By disabling the PermitRootLogin - remove direct root access entirely
     
   - Attackers must now:

      - Compromise a valid user account first
      - That user must also have sudo privileges
      - This reduces attack surface significantly.
    
        
 ### Why 777 is unsafe?
  - Using 777 means Owner ; group and other users can all read / write or execute in the file
  - which is dangerous because any user on the system can modify or delete the file, increasing risk of privilege escalation or malicious tampering.
  - Question demanded to give executable permission to all user - can be done by using 755
  - that is R,W,X for owner ; R,X for group and other users.
