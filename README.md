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


## 3. Now Create Internet GateWay for Your VPC: DNAT

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


## 5. NAT Gateway: SNAT

Basicallu NAT is ec2 instance launch in public subnet that subnet have internet gateway attched, NAT ec2 instance which is laucunched by AWS SO our private subnet services can go to outside and download packages.

Name: PS-wordpress-NAT_Gateway

<img width="1501" height="647" alt="image" src="https://github.com/user-attachments/assets/4b1cc4d9-2e6f-49d4-8425-2cc7cb997a60" />

NAT Gateway must launch in Public Subnet.

## 6. Route Table for NAT Gateway:

Here we create new route table for NAT Gateway

name:PS-Wordpress-RouteTable-NAT

Now here in Route table of NAT and "Subnet associations" we add Priavte Subnet which we will use for launch EC2 in private zone.

Subnet associations:
<img width="1287" height="273" alt="image" src="https://github.com/user-attachments/assets/2a069e82-8935-4c0f-a28c-bfa5c17afc7c" />

In Route we add use NAT for go outside:
<img width="1512" height="398" alt="image" src="https://github.com/user-attachments/assets/86d973a6-f5fb-44cf-a2af-84b3ef44a06a" />

- Note:
  We can launch NAT in multiple zone for HA.

## 7. Final VPC architecure for Mumbai Region:
<img width="1597" height="487" alt="image" src="https://github.com/user-attachments/assets/0b951425-4fef-4b2f-b225-b194251e330e" />

## 8. Launch EC2 in Private Subnet:(Full Secure)
EC2 name: WorkPress_Mumbai-region-EC2

During Launching ec2 the ec2 must in Private Subnet & no public accesss
<img width="927" height="349" alt="image" src="https://github.com/user-attachments/assets/58df273b-58d0-4db5-99f8-9e414d65cd80" />

In "Adanced details"- Now attch a IAM rule that allow ec2 ssm access (AmazonSSMManagedInstanceCore):
<img width="1090" height="237" alt="image" src="https://github.com/user-attachments/assets/8a3494fb-2208-422e-ab29-283255d052c3" />


Now connect to EC2 by SSM and chcek NAT is working or by ping to the goggle:
<img width="1295" height="146" alt="image" src="https://github.com/user-attachments/assets/199b44c7-bb2b-4445-9229-4c9e2e105976" />



# Sydney Region: DataBase Backend(Private)
This Region only used for host db in private ec2 No public connectivity or anything.

1. Create VPC:
Name: PS-DataBase-VPC
CIDR: 10.10.0.0/16

2. Create 4 subnet, Two public two private:
<img width="1312" height="152" alt="image" src="https://github.com/user-attachments/assets/0ee2146a-92c5-4841-8ea5-3c668f2d1320" />


3. Now create Internet gateway:

Here Public DNAY traffic not come in but inside traffic like install images need go outside So NAT is needed.

**NAT Gateway WILL NOT WORK without IGW, NAT Gateway must be in a public subnet**  So we need Internet gateway here also.




