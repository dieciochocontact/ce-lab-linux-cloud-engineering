# Lab M1.08 - Linux for Cloud Engineering - Solution

## Scenario 1: SSH Setup and Connection

Connected to EC2 instance using SSH with key-based authentication.

**Commands used:**
chmod 400 ~/claveoreja.pem
ssh -i ~/claveoreja.pem ubuntu@54.84.85.77

**SSH Config file (~/.ssh/config):**
Host my-server
    HostName 54.84.85.77
    User ubuntu
    IdentityFile ~/.ssh/claveoreja.pem

**Screenshot:** ssh-connection.png

## Scenario 2: System Exploration

**Commands used:**
uname -a
lsb_release -a
df -h
free -h
top
ip addr
netstat -tuln

**System Inventory:**
- OS: Ubuntu 26.04 LTS
- Kernel: 7.0.0-1004-aws
- Architecture: x86_64
- Disk: 8GB total, 6GB available
- Memory: 1GB total
- Network: eth0 with private IP 172.31.1.221

**Screenshot:** system-resources.png

## Scenario 3: Log Analysis

**Commands used:**
sudo tail -50 /var/log/syslog
sudo grep "error" /var/log/syslog
sudo grep "Failed password" /var/log/auth.log
sudo grep "ERROR" /var/log/syslog | cut -d' ' -f6 | sort | uniq -c

**Findings:**
- Nginx started successfully
- AppArmor denied access to a file (security alert)
- CRON jobs running normally
- No failed SSH authentication attempts found

**Screenshot:** log-analysis.png

## Scenario 4: Web Server Setup

**Commands used:**
sudo apt update
sudo apt install nginx -y
sudo systemctl start nginx
sudo systemctl enable nginx
sudo systemctl status nginx
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
sudo nginx -t
sudo systemctl reload nginx

**Result:** Nginx accessible via browser at http://54.84.85.77

**Screenshot:** nginx-browser.png

## Scenario 5: Resource Management

**Commands used:**
top
ps aux --sort=-%mem | head
ps aux --sort=-%cpu | head
df -h
du -sh /var/log/*
sudo apt clean
sudo apt autoremove
uptime

**Findings:**
- CPU usage: low (under 5%)
- Memory usage: 200MB of 1GB used
- Disk usage: 2GB of 8GB used
- No resource-heavy processes found

## Scenario 6: Environment Configuration

**Commands used:**
env
echo $PATH
export MY_VAR="cloud-lab"
echo 'export MY_VAR="cloud-lab"' >> ~/.bashrc
echo 'alias ll="ls -lah"' >> ~/.bashrc
source ~/.bashrc

**Result:** Custom environment variables and aliases configured successfully.

## Reflection Questions

1. **Most challenging part:** Configuring the network correctly (Internet Gateway, route tables, security groups) to allow SSH access.
2. **Remote vs local:** Working remotely requires SSH, no graphical interface, and careful security configuration.
3. **If SSH was lost during maintenance:** Use AWS Session Manager to reconnect without SSH.
4. **Automating repetitive tasks:** Use bash scripts or Terraform to automate server setup and configuration.

## EC2 Termination

Instance terminated after completing the lab to avoid ongoing charges.
Termination confirmed in EC2 console.
