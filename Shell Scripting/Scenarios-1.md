🧠 SECTION 1: Real-Time Shell Scripting Scenarios for DevOps
1. Monitoring disk space and sending alerts

Scenario:
Write a shell script to check disk usage. If usage exceeds 80%, send an alert email or Slack notification.

Sample logic:
```
#!/bin/bash
THRESHOLD=80
EMAIL="devops@company.com"

df -H | awk 'NR>1 {print $5 " " $6}' | while read output; do
  usage=$(echo $output | awk '{print $1}' | sed 's/%//')
  partition=$(echo $output | awk '{print $2}')
  
  if [ $usage -ge $THRESHOLD ]; then
    echo "CRITICAL: Disk usage on $partition is ${usage}%" | mail -s "Disk Alert: $partition" $EMAIL
  fi
done
```
#!/bin/bash
👉 Shebang: tells the system to execute this script using Bash.

THRESHOLD=80
EMAIL="devops@company.com"
👉 Defines the disk usage limit and alert recipient.

df -H
👉 Displays disk usage in human-readable format.

df -H | awk 'NR>1 {print $5 " " $6}'
👉 Skips header and prints usage percentage ($5) and mount point ($6).

while read output; do
👉 Reads each filesystem line one by one.

usage=$(echo $output | awk '{print $1}' | sed 's/%//')
👉 Extracts numeric usage value by removing %.

partition=$(echo $output | awk '{print $2}')
👉 Captures the mount point.

if [ $usage -ge $THRESHOLD ]; then
👉 Compares current usage with threshold.

echo "CRITICAL: Disk usage on $partition is ${usage}%"
👉 Prints alert message (can be emailed or sent to Slack).

fi
done
👉 Ends conditional and loop.

2. Checking if a process is running and restarting if stopped

Scenario:
Monitor a service (e.g., nginx) and restart if it’s not running.

Example:
```
#!/bin/bash
SERVICE="nginx"

if ! pgrep -x "$SERVICE" > /dev/null
then
  echo "$SERVICE not running, starting it..."
  systemctl start $SERVICE
else
  echo "$SERVICE is running."
fi
```
#!/bin/bash
SERVICE="nginx"
👉 Defines the service to monitor.

pgrep -x "$SERVICE" > /dev/null
👉 Checks if the exact process name exists.

if ! pgrep -x "$SERVICE" > /dev/null
👉 ! means process is NOT running.

systemctl start $SERVICE
👉 Automatically restarts the service.

else
echo "$SERVICE is running."
fi
👉 Prints service status.

3. Log file analysis

Scenario:
Find top 5 IPs hitting a web server.

Example:
```
#!/bin/bash
LOGFILE="/var/log/httpd/access.log"
echo "Top 5 IPs:"
awk '{print $1}' $LOGFILE | sort | uniq -c | sort -nr | head -5
```
awk '{print $1}' access.log
👉 Extracts the client IP from each log entry.

sort
👉 Sorts IPs to group duplicates.

uniq -c
👉 Counts number of requests per IP.

sort -nr
👉 Sorts results in descending order.

head -5
👉 Displays top 5 IPs.


4. Auto-backup & rotation

Scenario:
Create a daily backup of /etc to /backup with timestamp and retain last 7 days.

Example:
```
#!/bin/bash
SRC="/etc"
DEST="/backup/etc-$(date +%F).tar.gz"

tar -czf $DEST $SRC
find /backup -type f -mtime +7 -delete
```
tar -czf /backup/etc-$(date +%F).tar.gz /etc
👉 Compresses /etc with today’s date in filename.

find /backup -type f -mtime +7 -delete
👉 Deletes backup files older than 7 days.


5. Validate a list of URLs

Scenario:
You have a file with URLs; check which ones return 200.

Example:
```
#!/bin/bash
while read url; do
  status=$(curl -o /dev/null -s -w "%{http_code}" $url)
  if [ "$status" -eq 200 ]; then
    echo "$url is UP"
  else
    echo "$url is DOWN (Status: $status)"
  fi
done < urls.txt
```

5️⃣ URL Health Check Script
while read url; do
👉 Reads URLs from a file line by line.

curl -o /dev/null -s -w "%{http_code}" $url
👉 Sends HTTP request silently and captures status code.

if [ "$status" -eq 200 ]; then
👉 Checks if the service is healthy.

echo "$url is UP"
else
echo "$url is DOWN"
fi
done < urls.txt
👉 Prints result and reads input file.

6. Find large files in a directory

Scenario:
Identify top 10 largest files in /var.

Example:
```
du -ah /var | sort -rh | head -10
```
du -ah /var
👉 Calculates disk usage for all files.

sort -rh
👉 Sorts files by size (largest first).

head -10
👉 Displays top 10 largest files.


7. Extract failed login attempts

Scenario:
From /var/log/secure or /var/log/auth.log, extract all failed SSH logins.

Example:
```
grep "Failed password" /var/log/secure | awk '{print $(NF-3)}' | sort | uniq -c | sort -nr
```
grep "Failed password" /var/log/secure
👉 Filters failed login attempts.

awk '{print $(NF-3)}'
👉 Extracts IP address from log entry.

sort | uniq -c | sort -nr
👉 Counts and ranks attacking IPs.

8. Memory and CPU monitoring script

Scenario:
Alert if CPU > 80% or memory > 90%.

Example:
```
#!/bin/bash
cpu=$(top -bn1 | grep "Cpu(s)" | awk '{print 100 - $8}')
mem=$(free | awk '/Mem/{printf("%.0f"), $3/$2*100}')

if (( ${cpu%.*} > 80 || $mem > 90 )); then
  echo "High resource usage! CPU: $cpu%, MEM: $mem%"
fi
```
top -bn1
👉 Runs top in batch mode once.

awk '{print 100 - $8}'
👉 Calculates actual CPU usage.

free | awk '/Mem/{printf("%.0f"), $3/$2*100}'
👉 Calculates memory usage percentage.

if (( cpu > 80 || mem > 90 )); then
👉 Triggers alert when thresholds exceed.

9. Delete old log files

Scenario:
Remove .log files older than 15 days.
```
find /var/log -name "*.log" -type f -mtime +15 -exec rm -f {} \;
```
find /var/log -name "*.log" -type f -mtime +15
👉 Finds log files older than 15 days.

-exec rm -f {} \;
👉 Deletes matched files.

10. Verify SSH connectivity to list of servers

Scenario:
Read list of servers from file, check SSH connectivity.

Example:
```
#!/bin/bash
for host in $(cat servers.txt); do
  if ssh -o BatchMode=yes -o ConnectTimeout=5 $host "exit" 2>/dev/null; then
    echo "$host - Reachable"
  else
    echo "$host - Unreachable"
  fi
done
```
for host in $(cat servers.txt); do
👉 Iterates through server list.

ssh -o BatchMode=yes -o ConnectTimeout=5 $host "exit"
👉 Attempts password-less SSH with timeout.

&& echo "$host Reachable" || echo "$host Unreachable"
👉 Prints connectivity status.


| **Question**                                                  | **Expected Discussion / Key Point**                                                                       |   |                                  |
| ------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------- | - | -------------------------------- |
| How do you handle passing secrets in shell scripts?           | Use environment variables, `.env` files, or secret managers (Vault, AWS Secrets Manager). Never hardcode. |   |                                  |
| How do you debug a script that fails intermittently?          | Use `set -x`, `set -e`, `trap`, and log outputs.                                                          |   |                                  |
| How do you make your scripts idempotent?                      | Check before action (e.g., if file exists, if service is running).                                        |   |                                  |
| How do you schedule scripts?                                  | Using `cron`, `systemd timers`, or Jenkins scheduled jobs.                                                |   |                                  |
| What’s the difference between `"$var"`, `$var`, and `${var}`? | Quoting avoids word splitting and globbing. `${}` is preferred for clarity.                               |   |                                  |
| How do you handle error codes in scripts?                     | Check `$?`, use `                                                                                         |   | exit 1`, or wrap in `if` blocks. |
| What’s the use of `trap` in shell?                            | To catch signals (like `INT`, `EXIT`) and perform cleanup.                                                |   |                                  |
| How do you improve a slow shell script?                       | Replace loops with `awk`/`sed`, use parallelism, reduce external command calls.                           |   |                                  |
| How do you test your shell scripts?                           | Use `shellcheck` for static analysis; dry-run critical operations.                                        |   |                                  |
| How do you read input securely (like passwords)?              | Use `read -s var` (silent input).                                                                         |   |                                  |


⚡ SECTION 3: Real-Time Interview Coding Tasks

These are hands-on problems that interviewers often ask you to write or explain on the spot.

Reverse a string or a file content
```
echo "Hello" | rev
tac file.txt
```
Find the number of running processes by user
```
ps -ef | grep username | wc -l
```

Extract specific column from CSV
```
awk -F, '{print $2}' file.csv
```

Check if a file exists and non-empty
```
[ -s /tmp/data.txt ] && echo "File exists and not empty" || echo "Empty"
```

Ping a list of servers and log results
```
for host in $(cat hosts.txt); do
  ping -c1 $host &>/dev/null && echo "$host: UP" || echo "$host: DOWN"
done
```
🧩 SECTION 4: Tricky & Debugging Questions

A script runs fine manually but fails in cron. Why?
👉 Environment variables not loaded in cron. Use absolute paths or source profile.

rm command deletes wrong files. How to prevent this?
👉 Use -i interactive, or test with echo before actual deletion.

You wrote a script but it says command not found. Why?
👉 Missing shebang (#!/bin/bash) or script not executable (chmod +x script.sh).

Script fails with “bad substitution” error. Why?
👉 Using Bash syntax in /bin/sh shell. Always start with correct shebang.
