## Creating a user with non-interactive shell
`sudo useradd -r -s /sbin/nologin -M  <usr-name>`

- -r : Use to create a system account
- -s /sbin/nologin : create a non-interactive shell , politely refusing a login
- -M : skip the home directory


  ## Did the project-Discussion with claude : 
 ### When to Start the Project
  - After Day 10 of the Linux track (File Permission Correction). By then you'll have covered users, permissions, SSH, backup, cron, and secure transfer — that's enough to build something real and small that actually demonstrates skill.
  - Don't wait until you finish all 100 days. That's the trap most people fall into — "I'll build something when I know enough." You never feel like you know enough. Build while learning.

### What to Build and When — Specifically Mapped to Your Curriculum
  - After Linux Day 10 → First Project
  - Build a simple User Management Automation Script in bash. 
  - It should create a user with a non-interactive shell, set an expiry date, 
  - lock down SSH root access, and back up their home directory — basically 
  - automate everything you learned in tasks 1–8 into one script. 
  - This is directly what companies ask junior sysadmin/DevOps candidates to demo.
  - Put it on GitHub with a README that explains what it does and why each decision was made.
  - After Linux Day 18 (SELinux, Firewall, Cron) → Second Project
  - Build a Server Hardening Checklist Script — a bash script that audits a Linux
  -  server and reports what's configured correctly and what isn't. Covers firewall rules,
  -  SSH config, SELinux status, cron access restrictions. This is literally what sysadmins
  -  do in real jobs.After DevOps Day 8 (Ansible) → Third Project
  - Take your hardening script and convert it into an Ansible Playbook
  -  that automates the whole server setup from scratch. This is where your resume starts
  -  looking genuinely impressive because you're showing progression — first you scripted it
  -  manually, then you automated the automation.
  - After DevOps Day 13 (IPTables + Network Services) → Fourth Project
  - Set up a small home lab on AWS free tier — one EC2 instance, configure it from scratch
  -  using your Ansible playbook, set up firewall rules, configure SSH properly, add a cron job.
  -  Document the whole architecture. This shows you can work with real infrastructure, not just a lab environment.

## Documented
### made the notes on github, & posted on linkedin about the problem I faces . 
