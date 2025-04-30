# 🚀 Infrastructure Bootcamp Assignment  
**AWS Web Application Deployment with ALB + ASG + S3**  
Clarusway Bootcamp - Week 9 Assignment

## 🗂️ Project Overview

This project demonstrates the deployment of a highly available web application using:

- **Amazon S3** for static assets
- **Auto Scaling Group (ASG)** for web servers running NGINX
- **Application Load Balancer (ALB)** for distributing traffic

---

## 📁 Part 1: S3 Setup (Static Assets)

### ✅ Tasks Completed:

- Created bucket: `w7ed-clarusway-assets` in region `eu-north-1`
- Uploaded:
  - `index.html`
  - `logo.png`, `sda.png`
- Enabled static website hosting
- Applied public-read bucket policy

### 🔗 S3 Website URL:

http://w7ed-clarusway-assets.s3-website.eu-north-1.amazonaws.com/


### 🧪 Validation:

- ✅ Screenshot: `s3-website.png`
- ✅ `curl -I` Output:

HTTP/1.1 200 OK ...


---

## 🖥️ Part 2: Auto Scaling Group (ASG)

### ✅ Launch Template Configuration:

**User Data Script:**
```bash
#!/bin/bash
yum update -y
amazon-linux-extras enable nginx1
yum clean metadata
yum install nginx -y
systemctl start nginx
systemctl enable nginx
aws s3 cp s3://w7ed-clarusway-assets/index.html /usr/share/nginx/html/index.html

✅ ASG Setup:

    Min: 1, Max: 3, Desired: 2

    Health checks: EC2 + ELB

    Launch Template: clarusway-launch-template

🧪 Validation:

    ✅ Screenshot: asg-instances.png (2 running instances)

    ✅ Screenshot: asg-config.png (ASG configuration details)

🌐 Part 3: Application Load Balancer (ALB)
✅ ALB Configuration:

    Internet-facing

    HTTP listener on port 80

    Target Group: health check path /

    Instances from ASG automatically registered

🔗 ALB DNS:

http://clarusway-alb-123456789.eu-north-1.elb.amazonaws.com

🧪 Validation:

    ✅ Screenshot: alb-browser-access.png

    ✅ curl round-robin test:

for i in {1..5}; do curl -s http://clarusway-alb-123456789.eu-north-1.elb.amazonaws.com | grep "hostname"; done

✅ Success Criteria

    Website accessible via both:

        S3 Endpoint

        ALB DNS

    ASG replaces terminated instances automatically

    HTML and image assets load correctly in browser

🧹 Cleanup Done

    ✅ Deleted S3 bucket

    ✅ Terminated ASG

    ✅ Removed ALB and target group

🔐 S3 Bucket Policy Used

{
  "Version": "2012-10-17",
  "Statement": [{
    "Effect": "Allow",
    "Principal": "*",
    "Action": "s3:GetObject",
    "Resource": "arn:aws:s3:::w7ed-clarusway-assets/*"
  }]
}

📸 Screenshots

    s3-website.png

    asg-instances.png

    asg-config.png

    alb-browser-access.png

👨‍💻 Author

w7ed
Clarusway DevOps Bootcamp – Week 9
