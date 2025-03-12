---
# Host a Static Website on AWS

## Overview
This project demonstrates how to host a static HTML web application on AWS using a variety of AWS services for security, scalability, and fault tolerance. The web application is deployed on an EC2 instance within a Virtual Private Cloud (VPC) and is managed through an Auto Scaling Group with an Application Load Balancer.

## Architecture Components
1. **Virtual Private Cloud (VPC)**: Configured with both public and private subnets across two different availability zones.
2. **Internet Gateway**: Enables connectivity between the VPC instances and the internet.
3. **Security Groups**: Acts as a network firewall to regulate traffic.
4. **Availability Zones**: Two Availability Zones are used to improve reliability and fault tolerance.
5. **Public Subnets**: Utilized for infrastructure components like the NAT Gateway and Application Load Balancer.
6. **EC2 Instance Connect Endpoint**: Provides secure connections to resources in both public and private subnets.
7. **Private Subnets**: Hosts web servers (EC2 instances) for enhanced security.
8. **NAT Gateway**: Allows instances in private subnets to access the internet securely.
9. **EC2 Instances**: Hosts the static website.
10. **Application Load Balancer (ALB)**: Distributes incoming traffic to an Auto Scaling Group of EC2 instances across multiple Availability Zones.
11. **Auto Scaling Group (ASG)**: Ensures high availability, scalability, and fault tolerance by dynamically managing EC2 instances.
12. **GitHub**: Stores web files for version control and collaboration.
13. **AWS Certificate Manager**: Secures application communications with SSL/TLS.
14. **Simple Notification Service (SNS)**: Sends alerts regarding Auto Scaling Group activities.
15. **Route 53**: Manages the domain name and DNS records for the website.

## Deployment Instructions
### Prerequisites
- AWS account with access to EC2, VPC, Load Balancer, and Auto Scaling services.
- Registered domain name configured in Route 53.
- GitHub repository containing the website files.

### Deployment Steps
1. **Launch an EC2 Instance**:
   - Use Amazon Linux 2 as the AMI.
   - Assign an appropriate IAM role with permissions to access necessary AWS resources.
   - Place the instance in a private subnet.

2. **Execute the Initialization Script**
   - SSH into the EC2 instance and run the following commands:
   
   ```bash
   # Switch to root user
   sudo su
   
   # Update packages
   yum update -y
   
   # Install Apache HTTP Server
   yum install -y httpd
   
   # Change directory to Apache web root
   cd /var/www/html
   
   # Install Git
   yum install git -y
   
   # Clone the project GitHub repository
   git clone https://github.com/aosnotes77/host-a-static-website-on-aws.git
   
   # Copy files to the Apache web root
   cp -R host-a-static-website-on-aws/. /var/www/html/
   
   # Remove the cloned repository
   rm -rf host-a-static-website-on-aws
   
   # Enable and start Apache HTTP Server
   systemctl enable httpd
   systemctl start httpd
   
3. **Configure the Load Balancer**:
   - Create an Application Load Balancer in the public subnet.
   - Attach it to a target group associated with the EC2 instances.

4. **Set Up Auto Scaling**:
   - Configure an Auto Scaling Group with a launch template that includes the user-data script above.
   - Set scaling policies to automatically adjust the number of instances based on demand.

5. **Secure the Application**:
   - Use AWS Certificate Manager to enable HTTPS on the Load Balancer.
   - Apply Security Groups to restrict access to only necessary ports.

6. **Set Up Domain Name**:
   - Configure Route 53 with an alias record pointing to the Load Balancer.

7. **Enable Monitoring and Notifications**:
   - Use Amazon CloudWatch to monitor EC2 instances and Auto Scaling Group metrics.
   - Configure Amazon SNS for alerts regarding instance health and scaling events.

## Conclusion
By following these steps, a highly available, secure, and scalable static website is deployed on AWS. The architecture ensures fault tolerance through multiple Availability Zones, security through private subnets and firewalls, and automation with Auto Scaling and Load Balancing.

---
