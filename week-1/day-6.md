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
