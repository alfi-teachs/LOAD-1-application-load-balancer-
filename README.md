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

Click Include as pending

Create target group
