# Lab: Linux for Cloud Engineering - Advanced Practical Skills

**Estimated Time:** 120 minutes  
**Difficulty:** Intermediate  
**Prerequisites:** Linux Command Line Essentials lab completed, basic shell scripting knowledge

## Learning Objectives

By the end of this lab, you will be able to:
- Use advanced text processing tools (awk, sed, cut)
- Write practical shell scripts for cloud automation
- Process and analyze cloud service logs efficiently
- Work with JSON data using jq
- Implement log rotation and management strategies
- Apply Linux skills to real cloud engineering scenarios
- Automate repetitive cloud infrastructure tasks

## Scenario

You're a cloud engineer responsible for managing AWS infrastructure. Your daily tasks include analyzing CloudWatch logs, processing CloudTrail audit data, managing EC2 instance states, and automating deployments. This lab simulates real-world cloud engineering tasks that require advanced Linux skills.

## Lab Overview

You will:
1. Set up a cloud engineering workspace
2. Master advanced text processing (awk, sed, cut)
3. Parse and analyze JSON data with jq
4. Write automation scripts for cloud tasks
5. Implement log management strategies
6. Process real-world AWS CLI output
7. Create a deployment pipeline script

## Prerequisites

- Linux environment (EC2, local Linux, WSL, or macOS)
- AWS CLI installed (optional, we'll simulate output)
- Text editor (vi/vim, nano, or VS Code)
- Completed Linux Essentials lab

---

## Part 1: Advanced Text Processing

### Step 1: Working with awk

`awk` is a powerful text processing language perfect for structured data.

**Create sample server log:**
```bash
mkdir -p ~/cloud-labs/logs
cat > ~/cloud-labs/logs/server-access.log << 'EOF'
192.168.1.100 - - [14/Jan/2026:08:00:15] "GET /api/users HTTP/1.1" 200 1234
192.168.1.101 - - [14/Jan/2026:08:00:22] "POST /api/login HTTP/1.1" 200 567
192.168.1.102 - - [14/Jan/2026:08:00:35] "GET /api/products HTTP/1.1" 404 89
192.168.1.100 - - [14/Jan/2026:08:01:05] "GET /api/cart HTTP/1.1" 200 2345
192.168.1.103 - - [14/Jan/2026:08:01:23] "POST /api/orders HTTP/1.1" 500 123
192.168.1.101 - - [14/Jan/2026:08:02:11] "GET /api/users HTTP/1.1" 200 1567
192.168.1.104 - - [14/Jan/2026:08:02:45] "GET /api/products HTTP/1.1" 200 4532
192.168.1.102 - - [14/Jan/2026:08:03:12] "DELETE /api/cart/123 HTTP/1.1" 403 45
192.168.1.105 - - [14/Jan/2026:08:03:34] "GET /health HTTP/1.1" 200 12
192.168.1.100 - - [14/Jan/2026:08:04:22] "POST /api/checkout HTTP/1.1" 200 987
EOF
```

**awk basics:**
```bash
# Print specific columns (fields)
awk '{print $1}' ~/cloud-labs/logs/server-access.log  # IP addresses

# Print multiple columns
awk '{print $1, $9}' ~/cloud-labs/logs/server-access.log  # IP and status code

# Print with custom delimiter
awk '{print $1 " -> " $9}' ~/cloud-labs/logs/server-access.log

# Filter and print (status 200 only)
awk '$9 == 200 {print $0}' ~/cloud-labs/logs/server-access.log

# Calculate sum (total bytes transferred)
awk '{sum += $10} END {print "Total bytes:", sum}' ~/cloud-labs/logs/server-access.log

# Count occurrences
awk '{print $1}' ~/cloud-labs/logs/server-access.log | sort | uniq -c

# Average response size
awk '{sum += $10; count++} END {print "Average:", sum/count}' ~/cloud-labs/logs/server-access.log
```

**📋 Task 1:** Use awk to:
1. Extract all unique IP addresses
2. Count requests per IP
3. Calculate average response size
4. List all 4xx and 5xx errors

### Step 2: Working with sed

`sed` is a stream editor for text transformation.

**Create sample configuration:**
```bash
cat > ~/cloud-labs/config.txt << 'EOF'
server_name=production-web-01
port=8080
max_connections=100
timeout=30
enable_ssl=false
log_level=INFO
backup_enabled=true
backup_path=/var/backups
EOF
```

**sed operations:**
```bash
# Replace text
sed 's/production/staging/' ~/cloud-labs/config.txt

# Replace and save to new file
sed 's/production/staging/' ~/cloud-labs/config.txt > ~/cloud-labs/config-staging.txt

# Replace in place (-i flag)
sed -i.bak 's/INFO/DEBUG/' ~/cloud-labs/config.txt

# Delete lines matching pattern
sed '/backup/d' ~/cloud-labs/config.txt

# Print only matching lines
sed -n '/ssl/p' ~/cloud-labs/config.txt

# Replace multiple patterns
sed -e 's/8080/9090/' -e 's/false/true/' ~/cloud-labs/config.txt

# Replace on specific line number
sed '3s/100/200/' ~/cloud-labs/config.txt

# Add line after pattern
sed '/port/a new_setting=value' ~/cloud-labs/config.txt
```

**📋 Task 2:** Use sed to:
1. Create a dev configuration (change production to dev, port to 3000)
2. Enable SSL in the config
3. Change log level to ERROR

### Step 3: Working with cut

`cut` extracts specific fields from text.

**Create CSV data:**
```bash
cat > ~/cloud-labs/instances.csv << 'EOF'
instance_id,instance_type,region,state,cost_per_hour
i-0a1b2c3d4e5,t2.micro,us-east-1,running,0.0116
i-1b2c3d4e5f6,t3.small,us-west-2,stopped,0.0208
i-2c3d4e5f6g7,t3.medium,eu-west-1,running,0.0416
i-3d4e5f6g7h8,m5.large,us-east-1,running,0.096
i-4e5f6g7h8i9,t2.micro,ap-south-1,running,0.0116
i-5f6g7h8i9j0,t3.xlarge,us-west-2,stopped,0.1664
EOF
```

**cut operations:**
```bash
# Extract specific field (comma-delimited)
cut -d',' -f1 ~/cloud-labs/instances.csv  # Instance IDs

# Multiple fields
cut -d',' -f1,3,4 ~/cloud-labs/instances.csv  # ID, region, state

# Range of fields
cut -d',' -f1-3 ~/cloud-labs/instances.csv

# Exclude header and extract
tail -n +2 ~/cloud-labs/instances.csv | cut -d',' -f1,5

# Extract characters (not fields)
cut -c1-10 ~/cloud-labs/instances.csv
```

**📋 Task 3:** Use cut to extract:
1. All running instances (ID and type)
2. Total hourly cost of running instances

---

## Part 2: JSON Processing with jq

### Step 4: Install and Use jq

`jq` is essential for processing JSON from AWS CLI and APIs.

**Install jq (if not available):**
```bash
# macOS
brew install jq

# Amazon Linux / CentOS
sudo yum install jq -y

# Ubuntu / Debian
sudo apt-get install jq -y
```

**Create sample AWS JSON output:**
```bash
cat > ~/cloud-labs/ec2-instances.json << 'EOF'
{
  "Reservations": [
    {
      "Instances": [
        {
          "InstanceId": "i-0a1b2c3d4e5",
          "InstanceType": "t2.micro",
          "State": {"Name": "running"},
          "Tags": [{"Key": "Name", "Value": "web-server-01"}, {"Key": "Environment", "Value": "production"}]
        }
      ]
    },
    {
      "Instances": [
        {
          "InstanceId": "i-1b2c3d4e5f6",
          "InstanceType": "t3.small",
          "State": {"Name": "stopped"},
          "Tags": [{"Key": "Name", "Value": "app-server-01"}, {"Key": "Environment", "Value": "staging"}]
        }
      ]
    },
    {
      "Instances": [
        {
          "InstanceId": "i-2c3d4e5f6g7",
          "InstanceType": "t3.medium",
          "State": {"Name": "running"},
          "Tags": [{"Key": "Name", "Value": "db-server-01"}, {"Key": "Environment", "Value": "production"}]
        }
      ]
    }
  ]
}
EOF
```

**jq operations:**
```bash
# Pretty print JSON
jq '.' ~/cloud-labs/ec2-instances.json

# Extract specific field
jq '.Reservations[].Instances[].InstanceId' ~/cloud-labs/ec2-instances.json

# Filter by condition
jq '.Reservations[].Instances[] | select(.State.Name == "running")' ~/cloud-labs/ec2-instances.json

# Extract multiple fields
jq '.Reservations[].Instances[] | {id: .InstanceId, type: .InstanceType, state: .State.Name}' ~/cloud-labs/ec2-instances.json

# Get specific tag value
jq '.Reservations[].Instances[] | .Tags[] | select(.Key == "Name") | .Value' ~/cloud-labs/ec2-instances.json

# Count instances
jq '[.Reservations[].Instances[]] | length' ~/cloud-labs/ec2-instances.json

# Complex query: running production instances
jq '.Reservations[].Instances[] | select(.State.Name == "running") | select(.Tags[] | select(.Key == "Environment" and .Value == "production")) | .InstanceId' ~/cloud-labs/ec2-instances.json
```

**📋 Task 4:** Use jq to:
1. List all instance IDs and their names
2. Find all running instances
3. Count instances per environment
4. Extract instance IDs for production environment only

---

## Part 3: Shell Scripting for Cloud Automation

### Step 5: Create EC2 Status Check Script

Write a script to check EC2 instance status.

```bash
cat > ~/cloud-labs/scripts/check-instances.sh << 'EOF'
#!/bin/bash
# EC2 Instance Status Checker
# Author: Cloud Engineering Bootcamp
# Date: 2026-01-14

set -e  # Exit on error

# Colors for output
RED='\033[0;31m'
GREEN='\033[0;32m'
YELLOW='\033[1;33m'
NC='\033[0m' # No Color

# File to process
INSTANCES_FILE="$HOME/cloud-labs/ec2-instances.json"

echo "========================================="
echo " EC2 Instance Status Report"
echo "========================================="
echo ""

# Check if file exists
if [ ! -f "$INSTANCES_FILE" ]; then
    echo -e "${RED}Error: File not found: $INSTANCES_FILE${NC}"
    exit 1
fi

# Check if jq is installed
if ! command -v jq &> /dev/null; then
    echo -e "${RED}Error: jq is not installed${NC}"
    exit 1
fi

# Count total instances
TOTAL=$(jq '[.Reservations[].Instances[]] | length' "$INSTANCES_FILE")
echo "Total Instances: $TOTAL"

# Count by state
RUNNING=$(jq '[.Reservations[].Instances[] | select(.State.Name == "running")] | length' "$INSTANCES_FILE")
STOPPED=$(jq '[.Reservations[].Instances[] | select(.State.Name == "stopped")] | length' "$INSTANCES_FILE")

echo -e "${GREEN}Running: $RUNNING${NC}"
echo -e "${RED}Stopped: $STOPPED${NC}"
echo ""

# List running instances
echo "Running Instances:"
echo "------------------"
jq -r '.Reservations[].Instances[] | select(.State.Name == "running") | "  • \(.InstanceId) - \(.InstanceType) - \((.Tags[] | select(.Key == "Name") | .Value) // "N/A")"' "$INSTANCES_FILE"

echo ""

# List stopped instances
echo "Stopped Instances:"
echo "------------------"
jq -r '.Reservations[].Instances[] | select(.State.Name == "stopped") | "  • \(.InstanceId) - \(.InstanceType) - \((.Tags[] | select(.Key == "Name") | .Value) // "N/A")"' "$INSTANCES_FILE"

echo ""
echo "========================================="
echo " Report Generated: $(date)"
echo "========================================="
EOF

chmod +x ~/cloud-labs/scripts/check-instances.sh
```

**Run the script:**
```bash
~/cloud-labs/scripts/check-instances.sh
```

**📋 Task 5:** Enhance the script to:
1. Accept filename as command-line argument
2. Add error handling for empty file
3. Show cost summary (if cost data available)

### Step 6: Log Rotation Script

Create a script to manage application logs.

```bash
cat > ~/cloud-labs/scripts/rotate-logs.sh << 'EOF'
#!/bin/bash
# Log Rotation Script
# Rotates logs older than 7 days

LOG_DIR="$HOME/cloud-labs/logs"
ARCHIVE_DIR="$HOME/cloud-labs/logs/archive"
DAYS_TO_KEEP=7

# Create archive directory if doesn't exist
mkdir -p "$ARCHIVE_DIR"

echo "Starting log rotation..."
echo "Log directory: $LOG_DIR"
echo "Archive directory: $ARCHIVE_DIR"
echo "Keeping logs newer than $DAYS_TO_KEEP days"
echo ""

# Find and move old logs
find "$LOG_DIR" -maxdepth 1 -name "*.log" -type f -mtime +$DAYS_TO_KEEP | while read -r logfile; do
    filename=$(basename "$logfile")
    timestamp=$(date +"%Y%m%d-%H%M%S")
    archive_name="${filename%.log}-${timestamp}.log.gz"
    
    echo "Archiving: $filename -> $archive_name"
    
    # Compress and move
    gzip -c "$logfile" > "$ARCHIVE_DIR/$archive_name"
    
    # Remove original if compression successful
    if [ $? -eq 0 ]; then
        rm "$logfile"
        echo "  ✓ Archived successfully"
    else
        echo "  ✗ Archive failed"
    fi
done

echo ""
echo "Log rotation complete."

# Show archive contents
echo ""
echo "Archived logs:"
ls -lh "$ARCHIVE_DIR"
EOF

chmod +x ~/cloud-labs/scripts/rotate-logs.sh
```

**📋 Task 6:** Modify the script to:
1. Accept days to keep as argument
2. Send email notification (simulate with echo)
3. Generate rotation summary report

---

## Part 4: Real-World Cloud Engineering Tasks

### Step 7: CloudWatch Logs Analysis

Simulate analyzing CloudWatch Logs.

**Create sample CloudWatch log:**
```bash
cat > ~/cloud-labs/logs/cloudwatch-app.log << 'EOF'
2026-01-14T08:00:00.123Z INFO [RequestHandler] Request received: GET /api/users
2026-01-14T08:00:00.145Z DEBUG [Database] Query executed in 15ms
2026-01-14T08:00:00.150Z INFO [RequestHandler] Response sent: 200 OK (27ms)
2026-01-14T08:00:05.234Z INFO [RequestHandler] Request received: POST /api/login
2026-01-14T08:00:05.267Z WARN [Auth] Failed login attempt for user: admin
2026-01-14T08:00:05.270Z INFO [RequestHandler] Response sent: 401 Unauthorized (36ms)
2026-01-14T08:00:10.456Z INFO [RequestHandler] Request received: GET /api/products
2026-01-14T08:00:10.489Z DEBUG [Database] Query executed in 28ms
2026-01-14T08:00:10.495Z INFO [RequestHandler] Response sent: 200 OK (39ms)
2026-01-14T08:00:15.678Z ERROR [Database] Connection timeout after 5000ms
2026-01-14T08:00:15.680Z ERROR [RequestHandler] Request failed: Database unavailable
2026-01-14T08:00:15.682Z INFO [RequestHandler] Response sent: 500 Internal Server Error (5004ms)
2026-01-14T08:00:20.789Z INFO [HealthCheck] Health check passed
2026-01-14T08:00:30.890Z INFO [RequestHandler] Request received: GET /api/cart
2026-01-14T08:00:30.923Z DEBUG [Database] Query executed in 31ms
2026-01-14T08:00:30.930Z INFO [RequestHandler] Response sent: 200 OK (40ms)
EOF
```

**Analysis tasks:**
```bash
# Count by log level
grep -oE '(INFO|DEBUG|WARN|ERROR)' ~/cloud-labs/logs/cloudwatch-app.log | sort | uniq -c

# Find slow requests (>100ms)
awk -F'[()]' '$(NF-1) ~ /ms/ && $(NF-1)+0 > 100 {print $0}' ~/cloud-labs/logs/cloudwatch-app.log

# Extract and average response times
grep -oE '\([0-9]+ms\)' ~/cloud-labs/logs/cloudwatch-app.log | grep -oE '[0-9]+' | awk '{sum+=$1; count++} END {print "Avg response time:", sum/count, "ms"}'

# Find all errors with context
grep -B 1 -A 1 ERROR ~/cloud-labs/logs/cloudwatch-app.log

# Extract endpoint usage
grep -oE '(GET|POST|PUT|DELETE) [^ ]+' ~/cloud-labs/logs/cloudwatch-app.log | sort | uniq -c | sort -rn
```

**📋 Task 7:** Create a script `analyze-cloudwatch.sh` that:
1. Counts log entries by level
2. Finds requests slower than threshold
3. Lists all errors with timestamps
4. Generates a summary report

### Step 8: Cost Analysis Script

Process AWS cost data.

**Create sample cost report:**
```bash
cat > ~/cloud-labs/costs.csv << 'EOF'
Service,Region,Cost,Date
EC2,us-east-1,125.50,2026-01-01
S3,us-east-1,45.30,2026-01-01
RDS,us-west-2,89.75,2026-01-01
EC2,us-east-1,132.25,2026-01-02
S3,us-east-1,47.10,2026-01-02
RDS,us-west-2,89.75,2026-01-02
Lambda,us-east-1,12.50,2026-01-02
EC2,us-east-1,128.00,2026-01-03
S3,us-east-1,46.20,2026-01-03
RDS,us-west-2,89.75,2026-01-03
Lambda,us-east-1,15.30,2026-01-03
CloudFront,global,22.40,2026-01-03
EOF
```

**Analysis:**
```bash
# Total cost per service
tail -n +2 ~/cloud-labs/costs.csv | awk -F',' '{sum[$1] += $3} END {for (service in sum) print service, sum[service]}' | sort -k2 -rn

# Cost by region
tail -n +2 ~/cloud-labs/costs.csv | awk -F',' '{sum[$2] += $3} END {for (region in sum) print region, sum[region]}'

# Daily total cost
tail -n +2 ~/cloud-labs/costs.csv | awk -F',' '{sum[$4] += $3} END {for (date in sum) print date, sum[date]}' | sort

# Highest cost services
tail -n +2 ~/cloud-labs/costs.csv | sort -t',' -k3 -rn | head -5
```

**📋 Task 8:** Create `cost-report.sh` that generates:
1. Total cost by service
2. Cost trend analysis
3. Alerts for services exceeding threshold
4. Formatted report with recommendations

---

## Part 5: Advanced Scripting - Deployment Pipeline

### Step 9: Build a Simple Deployment Script

Create a comprehensive deployment script.

```bash
cat > ~/cloud-labs/scripts/deploy.sh << 'EOF'
#!/bin/bash
# Application Deployment Script
# Usage: ./deploy.sh [environment] [version]

set -euo pipefail  # Strict error handling

# Configuration
ENVIRONMENTS=("dev" "staging" "production")
LOG_FILE="$HOME/cloud-labs/logs/deploy-$(date +%Y%m%d-%H%M%S).log"

# Functions
log() {
    echo "[$(date '+%Y-%m-%d %H:%M:%S')] $*" | tee -a "$LOG_FILE"
}

error() {
    echo "[ERROR] $*" | tee -a "$LOG_FILE" >&2
    exit 1
}

validate_environment() {
    local env=$1
    if [[ ! " ${ENVIRONMENTS[@]} " =~ " ${env} " ]]; then
        error "Invalid environment. Must be one of: ${ENVIRONMENTS[*]}"
    fi
}

pre_deployment_checks() {
    log "Running pre-deployment checks..."
    
    # Check disk space
    local available=$(df -h / | awk 'NR==2 {print $4}')
    log "Available disk space: $available"
    
    # Check if application is running
    if pgrep -f "my-app" > /dev/null; then
        log "Application is currently running"
    else
        log "Application is not running"
    fi
    
    log "Pre-deployment checks passed"
}

backup() {
    local env=$1
    log "Creating backup for $env environment..."
    
    # Simulate backup
    local backup_dir="$HOME/cloud-labs/backups/$env-$(date +%Y%m%d-%H%M%S)"
    mkdir -p "$backup_dir"
    
    log "Backup created at: $backup_dir"
}

deploy() {
    local env=$1
    local version=$2
    
    log "Deploying version $version to $env..."
    
    # Simulate deployment steps
    log "  • Pulling code from repository..."
    sleep 1
    
    log "  • Installing dependencies..."
    sleep 1
    
    log "  • Running database migrations..."
    sleep 1
    
    log "  • Restarting application..."
    sleep 1
    
    log "Deployment completed successfully"
}

health_check() {
    log "Running health check..."
    
    # Simulate health check
    sleep 2
    
    log "Health check passed ✓"
}

# Main execution
main() {
    if [ $# -lt 2 ]; then
        echo "Usage: $0 <environment> <version>"
        echo "Environments: ${ENVIRONMENTS[*]}"
        exit 1
    fi
    
    local environment=$1
    local version=$2
    
    log "========================================="
    log " Deployment Started"
    log "========================================="
    log "Environment: $environment"
    log "Version: $version"
    log ""
    
    validate_environment "$environment"
    pre_deployment_checks
    backup "$environment"
    deploy "$environment" "$version"
    health_check
    
    log ""
    log "========================================="
    log " Deployment Successful!"
    log "========================================="
    log "Log file: $LOG_FILE"
}

# Run main function
main "$@"
EOF

chmod +x ~/cloud-labs/scripts/deploy.sh
```

**Test the script:**
```bash
~/cloud-labs/scripts/deploy.sh staging v1.2.3
```

**📋 Task 9:** Enhance deployment script with:
1. Rollback capability
2. Slack/email notifications (simulated)
3. Post-deployment smoke tests
4. Deployment approval for production

---

## Verification Checklist

- [ ] Completed awk text processing tasks
- [ ] Used sed for configuration management
- [ ] Extracted data with cut
- [ ] Processed JSON with jq
- [ ] Created EC2 status check script
- [ ] Implemented log rotation script
- [ ] Analyzed CloudWatch logs
- [ ] Generated cost analysis
- [ ] Built deployment pipeline script
- [ ] All scripts executable and working

---

## Cleanup

```bash
# Optional: Keep for reference or remove
# rm -rf ~/cloud-labs
```

---

## Key Takeaways

✅ **awk** is perfect for columnar data analysis  
✅ **sed** enables powerful text transformations  
✅ **jq** is essential for JSON processing in cloud  
✅ **Shell scripts** automate repetitive cloud tasks  
✅ **Log analysis** skills are crucial for troubleshooting  
✅ **Automation** reduces errors and saves time  

---

## Next Steps

- Write more advanced scripts for your specific cloud tasks
- Learn about CI/CD pipelines (Jenkins, GitLab CI, GitHub Actions)
- Explore Infrastructure as Code (Terraform, CloudFormation)
- Study container orchestration (Docker, Kubernetes)
- Practice AWS CLI scripting for real infrastructure

**Congratulations!** You've mastered Linux for cloud engineering! 🚀
