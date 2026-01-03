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


Note: ALL subnets are private no one is public, The subnet which are attach to internet gateway that will Public subnet menas allow to client come in.


## 3. Now Create Internet GateWay for Your VPC:

Name: PS-Wordpress-InternetGateway

After Create Internet Gateway then Go to "Action" Button and attch The VPC(PS-Workpress-vpc-mumbai)

## 4. Create Route Table For Internet gateway:

name route tabel: PS-Wordpress-RouteTable-internetGateway, Then attach the VPC to route table.

Once create Route Table in "Subnet associations" Attch the subnet that we want to make public.

Explicit subnet associations 
<img width="1256" height="272" alt="image" src="https://github.com/user-attachments/assets/5365ee95-f512-4123-95b4-687290988e01" />

- Note: Local Route means it is switch like set-up, the ec2 in same vpc can communicate each other by using this.

Here Route enable means from internet gateway outside traffic allow using route table to come in only the subnet associated to the this Route tabel that is Public subnet. 
<img width="1445" height="287" alt="image" src="https://github.com/user-attachments/assets/13c52e88-d0e6-4855-9ec9-ad23873f704b" />


## 5. 



