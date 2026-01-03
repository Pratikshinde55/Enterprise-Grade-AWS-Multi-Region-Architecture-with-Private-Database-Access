# Enterprise-Grade-AWS-Multi-Region-Architecture-with-Private-Database-Access
DevOps Portfolio Platform on AWS using ALB, Private VPC, and VPN

# Mumbai Region- Wokrpress
## 1. Create VPC: (PS-Workpress-vpc-mumbai)
VPC CIDR: 10.10.0.0/16

## 2.  Now create 4 Subnets for HA: 2 Public 2 Private
Create Subnet using "PS-Workpress-vpc-mumbai" VPC.

Now create 4 subnets with diff available zones for HA & Fault tolerance:

  - PS-Workpress-vpc-mumbai-Pivate-1a (CIDR: 10.0.1.0/24) aps1-az3 (ap-south-1a)
  - PS-Workpress-vpc-mumbai-Pivate-1b (CIDR: 10.0.2.0/24) aps1-az3 (ap-south-1b))
  - PS-Workpress-vpc-mumbai-Public-1a (CIDR: 10.0.10.0/24) aps1-az1 (ap-south-1a)
  - PS-Workpress-vpc-mumbai-Public-1a (CIDR: 10.0.11.0/24) aps1-az3 (ap-south-1b)




## 3. Now Create Internet GateWay for Your VPC:

Name: PS-Wordpress-InternetGateway

After Create Internet Gateway then Go to "Action" Button and attch The VPC(PS-Workpress-vpc-mumbai)
