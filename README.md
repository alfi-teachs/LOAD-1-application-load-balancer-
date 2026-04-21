# LOAD-1-application-load-balancer-
# Step 1: Create EC2 Instances
Launch 2 Linux EC2 instances (Amazon Linux recommended).
Choose:
VPC: Default
Auto-assign Public IP: Enabled (only for initial setup)
Security Group: Allow HTTP (80) and SSH (22)

# Step 2: Install Nginx on Both Instances

Connect to each instance and run:
```bash 
sudo yum update -y
```
```bash 
sudo yum install nginx -y
```
```bash 
sudo systemctl start nginx
```
```bash 
sudo systemctl enable nginx
```
