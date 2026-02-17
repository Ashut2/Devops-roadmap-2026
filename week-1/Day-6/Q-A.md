
## Question Answers
### Q1 Journalctl Basics : where does it read from ?
- reads from systemd's journal
- systemd's journal is a binary log system that stores logs from kernel , services , session events in /var/log/jounral (or/run/log/journal for volatile storage)

### What -p err Does ?
- It a flag which filters logs by priority
- show only "error" (level 3 ) and higher severity messages , alerts , notice etc

### Difference: -n vs --since
-  `-n 20` will fetch most 20 recent logs from journal regardless of time .
-   `--since "30 minutes ago" ` will fetch log starting from that time .

### Service Logs Value
- Logs for a specific service like nginx (-u nginx) isolate
- issues to that app, ignoring system-wide noise—key for quick debugging, 
- like spotting web server crashes without sifting kernel or cron spam.


## How you would debug “nginx not starting”
### Quick Structure
If an interviewer asks "How would you debug nginx not starting?", respond confidently with a step-by-step process—show logical thinking from symptoms to root cause. Start verbalizing like: "I'd approach it systematically using systemd tools..."

### Sample Response Script
"I'd start by checking the service status: sudo systemctl status nginx to see if it's failed and grab the error code or recent logs. Then, dive deeper with journalctl -u nginx -n 50 for full logs, looking for patterns like config errors or port issues.
​

Next, validate the config: sudo nginx -t catches most syntax problems in /etc/nginx/nginx.conf. If that passes, check common culprits—ss -tlnp | grep :80 for port conflicts, file perms on /var/log/nginx, or resources with free -h. 

Finally, daemon-reload, restart, and tail logs live: journalctl -u nginx -f. This covers 90% of cases quickly."


### Debug Sequence
Here's the exact order of commands to debug "nginx not starting"—run them one by one on a test box to memorize.

- sudo systemctl status nginx – See overall status, exit code, PID, and snippet of error.
​

- sudo journalctl -u nginx -n 50 – Full recent logs for the smoking gun.
​

- sudo nginx -t – Test config syntax; fix any errors in /etc/nginx/.

- sudo ss -tlnp | grep :80 (or :443) – Check for port conflicts. 
​

- ls -la /var/log/nginx /var/www/html – Verify perms (owned by www-data/nginx user).
​

- sudo systemctl daemon-reload – Reload systemd after config changes.
​

- sudo systemctl restart nginx – Attempt restart.

- sudo journalctl -u nginx -f – Tail live logs to watch for new fails.

## scenario 
If someone says:

### “Server is slow”

What would you check first and why?

Explain:

- Load average

- Memory usage

- Disk usage

- CPU wait


**First Check**  
I'd start with load average using `uptime` or `top`. It gives a quick snapshot—if it's higher than your CPU cores (like 4.2 on a 4-core server), the system's overloaded and everything feels slow.  

#### Load Average  
`uptime` shows 1/5/15-min averages. High numbers mean too many tasks queued; drill into `top` to see which processes (like nginx or mysql) are culprits. Practical: If load spikes during peak hours, scale up or kill hogs.
#### Memory Usage  
Run `free -h` next. Look at "available" column—if under 20% free or heavy swap use, apps are thrashing (paging to disk). Fix: Add RAM or tune caches; I've seen WordPress servers crawl from OOM kills.

#### Disk Usage  
`df -h` for space (`/var/log` filling up?), then `iotop` for I/O wait. Full disks or slow reads/writes (high %wa in `top`) bottleneck everything—logs or DBs eating space kill performance. 

#### CPU Wait  
In `top`, check %wa (I/O wait)—high means CPU idles waiting for disk/network. Not pure CPU usage (%us/sy), but blocked CPU; pair with `iotop` to find slow DB queries or NFS mounts. 

## Extra Questions
### What does systemctl status actually show?

**Systemctl Status Output**  
`systemctl status` shows a service snapshot: loaded state (from unit file), active/inactive status with color dots (green=good, red=bad), uptime, main PID, memory/CPU use, and the last 10-ish log lines for quick diagnosis.  

What It Displays  
Includes unit file path, "Active: active (running)" or "failed", process ID, resource stats, and recent journal logs—everything in one screen without digging further. 

### Where does it pull logs from?

Log Source  
Pulls logs directly from systemd's journal (same as `journalctl -u servicename`), querying the binary logs in `/var/log/journal/` for that service only.  

### What does “active (running)” vs “inactive (dead)” mean?

Active vs Inactive  
"Active (running)" means the service process is up, responding, and healthy. "Inactive (dead)" means systemd stopped it cleanly (or it never started)—check logs for why, like config fails or manual `stop`.
