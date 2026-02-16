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
​
