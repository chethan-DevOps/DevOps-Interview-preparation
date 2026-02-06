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
