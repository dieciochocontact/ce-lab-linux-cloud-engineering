# Lab Solution: Linux for Cloud Engineering

**Student Name:** ___________________________  
**Date:** ___________________________  
**Lab Completion Time:** ___________ minutes

---

## Part 1: Advanced Text Processing with awk

### Task 1: Server Log Analysis

**1. Extract unique IP addresses:**
```bash
# Command:

# Result:
_____________________________________________________________
_____________________________________________________________
```

**2. Count requests per IP:**
```bash
# Command:

# Result (top 3):
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**3. Calculate average response size:**
```bash
# Command:

# Result:
_____________________________________________________________
```

**4. List all 4xx and 5xx errors:**
```bash
# Command:

# Result:
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**Screenshot 1: awk analysis results**
![awk results](screenshots/01-awk-analysis.png)

---

## Part 2: Text Transformation with sed

### Task 2: Configuration Management

**1. Create dev configuration:**
```bash
# Commands:


# Verification (first 3 lines of new config):
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**2. Enable SSL:**
```bash
# Command:

# Result:
_____________________________________________________________
```

**3. Change log level to ERROR:**
```bash
# Command:

# Result:
_____________________________________________________________
```

**Screenshot 2: sed transformations**
![sed results](screenshots/02-sed-config.png)

---

## Part 3: Field Extraction with cut

### Task 3: Instance Data Processing

**1. Extract running instances (ID and type):**
```bash
# Command:

# Result:
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**2. Calculate total hourly cost of running instances:**
```bash
# Command:

# Result: $_____________ /hour
```

**Screenshot 3: cut operations**
![cut results](screenshots/03-cut-extraction.png)

---

## Part 4: JSON Processing with jq

### Task 4: AWS Instance JSON Analysis

**1. List all instance IDs and names:**
```bash
# Command:

# Result:
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**2. Find all running instances:**
```bash
# Command:

# Result:
_____________________________________________________________
_____________________________________________________________
```

**3. Count instances per environment:**
```bash
# Command:

# Result:
Production: _____
Staging: _____
```

**4. Production instance IDs only:**
```bash
# Command:

# Result:
_____________________________________________________________
_____________________________________________________________
```

**Screenshot 4: jq JSON processing**
![jq results](screenshots/04-jq-processing.png)

---

## Part 5: EC2 Status Check Script

### Task 5: Script Enhancement

**Enhanced script location:** ___________________________

**New features added:**
- [ ] Command-line argument for filename
- [ ] Error handling for empty file
- [ ] Cost summary calculation

**Script execution output:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**Screenshot 5: Enhanced EC2 status script**
![EC2 script](screenshots/05-ec2-status-script.png)

---

## Part 6: Log Rotation Script

### Task 6: Log Rotation Enhancements

**Modified script location:** ___________________________

**Enhancements made:**
- [ ] Days to keep as argument
- [ ] Email notification simulation
- [ ] Rotation summary report

**Script execution:**
```bash
# Command:

# Output:
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**Screenshot 6: Log rotation script output**
![Log rotation](screenshots/06-log-rotation.png)

---

## Part 7: CloudWatch Logs Analysis

### Task 7: Analysis Script

**Script name:** ___________________________

**Script content:**
```bash
#!/bin/bash
# CloudWatch Log Analyzer







```

**Execution results:**

**1. Log entries by level:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**2. Slow requests (>100ms):**
```
_____________________________________________________________
_____________________________________________________________
```

**3. All errors with timestamps:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**4. Summary report:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**Screenshot 7: CloudWatch analysis script**
![CloudWatch analysis](screenshots/07-cloudwatch-analysis.png)

---

## Part 8: Cost Analysis Script

### Task 8: Cost Reporting

**Script name:** ___________________________

**1. Total cost by service:**
```
Service          Total Cost
_______________________________
EC2              $_____________
S3               $_____________
RDS              $_____________
Lambda           $_____________
CloudFront       $_____________
```

**2. Cost trend:**
```
Date         Total Cost    Change
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**3. Services exceeding threshold (>$100):**
```
_____________________________________________________________
_____________________________________________________________
```

**4. Recommendations:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**Screenshot 8: Cost analysis report**
![Cost report](screenshots/08-cost-report.png)

---

## Part 9: Deployment Pipeline Script

### Task 9: Deployment Script Enhancements

**Enhanced script location:** ___________________________

**New features:**
- [ ] Rollback capability
- [ ] Notifications (simulated)
- [ ] Post-deployment smoke tests
- [ ] Production approval workflow

**Test deployment output:**
```bash
# Command:
./deploy.sh staging v1.2.3

# Output (first 10 lines):
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**Rollback test:**
```bash
# Command:

# Output:
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**Screenshot 9: Deployment script execution**
![Deployment](screenshots/09-deployment-script.png)

---

## Code Repository

**All scripts location:** ___________________________

**Script inventory:**
1. _______________________________________________
2. _______________________________________________
3. _______________________________________________
4. _______________________________________________
5. _______________________________________________

---

## Reflection Questions

### 1. When would you use awk vs sed vs cut for text processing?

**Your answer:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

### 2. Why is jq essential for cloud engineering with AWS CLI?

**Your answer:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

### 3. What are the benefits of shell scripts over manual commands?

**Your answer:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

### 4. How does log rotation prevent disk space issues?

**Your answer:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

### 5. Describe a real deployment pipeline you'd build for a cloud application.

**Your answer:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

---

## Advanced Challenges Completed

- [ ] Created multi-stage deployment pipeline
- [ ] Implemented automated rollback
- [ ] Added comprehensive error handling
- [ ] Built monitoring dashboard script
- [ ] Integrated with cloud APIs

**Notes on advanced work:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

---

## Script Quality Assessment

**For each major script, evaluate:**

| Script | Functionality | Error Handling | Documentation | Code Quality |
|--------|--------------|----------------|---------------|--------------|
| EC2 Status Check | ___/5 | ___/5 | ___/5 | ___/5 |
| Log Rotation | ___/5 | ___/5 | ___/5 | ___/5 |
| CloudWatch Analyzer | ___/5 | ___/5 | ___/5 | ___/5 |
| Cost Report | ___/5 | ___/5 | ___/5 | ___/5 |
| Deployment Pipeline | ___/5 | ___/5 | ___/5 | ___/5 |

---

## Troubleshooting Log

**Issues encountered:**

| Issue | Script/Tool | Solution | Time |
|-------|------------|----------|------|
|       |            |          |      |
|       |            |          |      |
|       |            |          |      |

---

## Self-Assessment

**Rate your mastery (1-5):**

| Skill | Before Lab | After Lab | Growth |
|-------|-----------|-----------|--------|
| awk text processing | ___/5 | ___/5 | +___ |
| sed transformations | ___/5 | ___/5 | +___ |
| jq JSON processing | ___/5 | ___/5 | +___ |
| Shell scripting | ___/5 | ___/5 | +___ |
| Log analysis | ___/5 | ___/5 | +___ |
| Automation mindset | ___/5 | ___/5 | +___ |
| Cloud operations | ___/5 | ___/5 | +___ |

---

## Real-World Applications

**How will you apply these skills in your cloud engineering work?**

1. _______________________________________________________________
2. _______________________________________________________________
3. _______________________________________________________________
4. _______________________________________________________________
5. _______________________________________________________________

---

## Instructor Verification

**Instructor Name:** ___________________________

**Date Reviewed:** ___________________________

**All scripts tested:** ☐ Yes ☐ No

**Code quality:** ☐ Excellent ☐ Good ☐ Needs Improvement

**Comments:**
```
_____________________________________________________________
_____________________________________________________________
_____________________________________________________________
```

**Grade/Status:** ___________________________

---

**Lab Status:** ☐ Complete ☐ Needs Revision

**Total Time Spent:** ________ minutes

**Submission Date:** ___________________________
