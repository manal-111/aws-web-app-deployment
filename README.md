# AWS Web Application Deployment

This repository contains the solution for the Week 9 assignment: AWS Web Application Deployment with ALB, ASG, and S3.

## Scenario

The assignment involves deploying a website using S3 for static assets, an Auto Scaling Group (ASG) for Nginx web servers, and an Application Load Balancer (ALB) for traffic distribution.

## Part 1: S3 Setup (Static Assets)

1.  **Create S3 Bucket:**
    * I created an S3 bucket named `yourname-clarusway-assets` in the `eu-north-1` region.


2.  **Upload Files:**
    * Uploaded `index.html`, `logo.png`, and `sda.png` to the bucket.

3.  **Configure Static Website Hosting:**
    * Enabled static website hosting for the bucket.
    * Index document: `index.html`

4.  **Set Bucket Policy:**
    * Applied the following bucket policy to allow public read access:

        ```json
        {
            "Version": "2012-10-17",
            "Statement": [{
                "Effect": "Allow",
                "Principal": "*",
                "Action": "s3:GetObject",
                "Resource": "arn:aws:s3:::yourname-clarusway-assets/*"
            }]
        }
        ```


5.  **Deliverables:**
    * S3 Website URL screenshot:  
    * `curl` output: 

## Part 2: Auto Scaling Group

1.  **Create Launch Template:**
    * Security group:  (Describe your security group rules)
    * User Data script:

        ```bash
        #!/bin/bash
        yum update -y
        yum install nginx -y
        systemctl start nginx
        systemctl enable nginx
        # (If you modified it to include hostname, add that here!)
        aws s3 cp s3://yourname-clarusway-assets/index.html /usr/share/nginx/html/
        ```

2.  **Configure Auto Scaling Group:** 
    * Desired capacity: 2, Min: 1, Max: 3
    * Health checks: EC2

3.  **Deliverables:**
    * Running instances screenshot:[ Instances.png](https://github.com/manal-111/aws-web-app-deployment/blob/aa5d50eb48723818b712ecebb0c65e4a46dcd40c/Instances.png)
    * ASG configuration details:

## Part 3: Application Load Balancer

1.  **Create Internet-facing ALB:**
    * Created an internet-facing Application Load Balancer named `clarusway-alb`.
    * Listener: HTTP, Port 80
    * Target group: `clarusway-target-group` (Instances, HTTP:80, Health checks: /)

2.  **Verify:**
    * Accessed the website via the ALB DNS name.
    * Verified round-robin traffic distribution using `curl`.



## Cleanup

I have deleted all the resources to avoid charges:

* Deleted the S3 bucket 
* Terminated the ASG
* Removed the ALB 


