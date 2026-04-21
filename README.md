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

# Step 3: Host Simple Website

Edit default page:

```bash
sudo vi /usr/share/nginx/html/index.html
```

Add content (different on each instance to test load balancing):

Instance 1

```bash
<h1>This is Server 1</h1>
```


Instance 2

```bash
<h1>This is Server 2</h1>
```

# Step 4: Test Using Public IP

Copy each instance Public IP

Paste in browser

You should see:

Server 1 page

Server 2 page


# Step 5: Create Target Group

Go to EC2 → Target Groups → Create

Name: tg1

Target type: Instance

Protocol: HTTP

Port: 80

IP version: IPv4

VPC: Default

Protocol version: HTTP1

Health Check

Protocol: HTTP

Path: /

Register Targets

# Select both EC2 instances

# Click Include as pending

Create target group

# Step 6: Create Application Load Balancer

Go to EC2 → Load Balancers → Create

# Type: Application Load Balancer

Name: ap-lb

Scheme: Internet-facing

IP type: IPv4

VPC: Default

Availability Zones

# Select at least 2 AZs

# Must match your instance AZs

Listener
Protocol: HTTP
Port: 80
Target Group
Select: tg1

Keep other settings default and create.

# Step 7: Configure Security Groups (Important Step)

For Load Balancer Security Group 

Allow:

HTTP (Port 80) → Source: Anywhere (0.0.0.0/0)

For EC2 Instance Security Group

Remove HTTP access from Anywhere

Add:

HTTP (Port 80)

Source: Load Balancer Security Group

👉 This ensures:

Instances are not directly accessible

Traffic only comes through ALB


# Step 8: Test Load Balancer

Copy DNS name of ALB

Paste in browser

Refresh multiple times → You should see:

Server 1

Server 2 (alternating)

Server 2 (alternating)

# What This Really Means

Your instances are now private behind ALB

Users access only through Load Balancer DNS

Traffic is distributed automatically

Health checks ensure only healthy servers receive traffic
