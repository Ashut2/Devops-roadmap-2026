# Interview questions related to jobs
## Answer these in your own words after testing:
ctrl + z sends a SIGTTP(terminal stop)  signal  to the  fg process . which suspends it but don't terminate it. The process is paused & moved to te badkground in a "stopped"
state. 
## What is the difference between Ctrl+Z and kill?
- Kill  sends a SIGTERM (signal 15 : Polite Termination Request) to the process for gracefully terminate
  & perform cleanup operation . 
- It releases all resources or clean files , Save state  & then exit cleanly
- ctrl + z  sends  a SIGTSTP ( Terminate Stop) signal to foreground process, which suspends it but doesn't terminate it.
- we can resume these stopped processes using fg & bg process.
## What does kill send by default?
SIGTERM ( signal 15) is the default signal  
## Why is SIGTERM preferred over SIGKILL?
### Cause SIGTERM asks process to terminaate gracefully, this signal also known as Polite Termination Request.
-  The process can catch the signal & perform cleanup operations
-  temporary files are deleted
-  database transaction can be commited or roll back
-  network connections can be closed properly
-  child process can be notified.
### SIGKILL(KILL -9) is forceful & immediate
- cannot be caught ,blocked or ignored
- no chance for the application to save state.
- can leave files corrupted , locks held,

  EG: if you sigterm a database , it can flush buffers & ensure data integrity.
  but sigkill might corrupt it .
  

## When would you use kill -9?
We will only use it as an emergency exit option , when 
- process is unrespinsive
- Won't respond to SIGTERM
- process is stuck : uninterruptable state
- Emergency : system is short on resources & we need immediate termination.
- After SIGTERM fails - You've tried kill <PID>, waited reasonable time, and the process is still running

## What is the difference between a background job and a daemon/service?

- bg job started with shell session( & , bg cmd) but daemon runs independently of any terminal.
- bg job dies with the terminal but daemon persists across login sessions
- bg managed by shell  (jobs,fg,bg cmds) but daemon managed by systemctl , services
- bg job is temoporary & user specific but daemon is system-wide , often runs as dedicated.
- EG : bg job = sleep 500 & , daemon : cron ,systemd-journald etc

# Day-5 kpi's done
- revise renice : renice -n 10 -p pid
- vmstat - report systmer performance statistics
- free -m - show memory usage
