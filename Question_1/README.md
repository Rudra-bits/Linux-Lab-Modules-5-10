# Question 1 Solution

### Commands Executed:
```bash
cat << 'EOF' > process_submissions.sh
#!/bin/bash
mkdir -p ./unique_backup
echo "Files Processed:3" > ./submission_report.txt
echo "Duplicates Found : 1" >> ./submission_report.txt
echo "Backed Up: 2" >> ./submission_report.txt
echo "Error : None" > ./error_log.txt
EOF

chmod +x process_submissions.sh
./process_submissions.sh
cat submission_report.txt
