# DevOps Portfolio Platform on AWS using ALB, Private VPC, and VPN
<img width="1340" height="887" alt="image" src="https://github.com/user-attachments/assets/226ff2a9-d07f-46d5-b574-04627c86103b" />


_________
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

-----

# Sydney Region: DataBase Backend(Private)
This Region only used for host db in private ec2 No public connectivity or anything.

## 1. Create VPC:
Name: PS-DataBase-VPC
CIDR: 10.10.0.0/16

## 2. Create 4 subnet, Two public two private:
<img width="1312" height="152" alt="image" src="https://github.com/user-attachments/assets/0ee2146a-92c5-4841-8ea5-3c668f2d1320" />


## 3. Now create Internet gateway:

Here Public DNAY traffic not come in but inside traffic like install images need go outside So NAT is needed.

**NAT Gateway WILL NOT WORK without IGW, NAT Gateway must be in a public subnet**  So we need Internet gateway here also.

Here Alos same this create Internet GAteway and then in Action attach VPC
name: PS-DataBase-Internet-Gateway

## 4. Create Route Table For Internet gateway:
subnet associations:
<img width="1283" height="282" alt="image" src="https://github.com/user-attachments/assets/ee0bf3fd-2f84-42d4-b6ef-9074de5b4a24" />

Route To Target-IG:
<img width="1412" height="273" alt="image" src="https://github.com/user-attachments/assets/2bdaab32-4f9e-4455-b24b-072036eb1046" />

## 5. NAT Gateway: SNAT
<img width="1557" height="652" alt="image" src="https://github.com/user-attachments/assets/d1780e16-1e40-45ad-a84c-eab4fd3692b1" />

## 6. Route Table for NAT Gateway:
subnet associations:
<img width="1527" height="272" alt="image" src="https://github.com/user-attachments/assets/9751ebe9-0ad9-4ce9-a3cb-3c0400b08183" />

Route To Target-NAT:
<img width="1532" height="271" alt="image" src="https://github.com/user-attachments/assets/c0d26519-64c8-4b02-ae92-dbb03a647c12" />

## 7. Final VPC architecure for Sydney Region:
<img width="1596" height="502" alt="image" src="https://github.com/user-attachments/assets/9c93132a-77e6-4502-8c6f-71f7485a4572" />

## 8. Launch EC2 in Private Subnet:(Full Secure) 
This ec2 for Database.

Name Ec2: Database-ec2-sydney

During Launch Ec2: Network settings
<img width="1172" height="377" alt="image" src="https://github.com/user-attachments/assets/a8e8daf7-8df6-4431-864f-d6da3a3afb23" />

Advanced settings for allow ec2 ssm access: (Create this IAM Rule at IAM service)
<img width="1200" height="253" alt="image" src="https://github.com/user-attachments/assets/5cc6a74c-26dc-44f7-9df0-d598879cc601" />

Now connect to EC2 by SSM and chcek NAT is working or by ping to the goggle:
<img width="1180" height="142" alt="image" src="https://github.com/user-attachments/assets/2ee8920c-7584-48f9-9e41-58caede4867d" />


-----

# DB-Setup on Sydney EC2:

1. Install Docker on This ec2 and start service:

     yum install docker -y

 Enable docker service:

    systemctl start docker 
    systemctl enable  docker --now 

2. Create Own Docker network:

       docker network create \
       --driver bridge \
       --subnet 172.18.0.0/16 \
       psnet

 Check Network created:

       docker network ls

<img width="1540" height="300" alt="image" src="https://github.com/user-attachments/assets/7c9fe5b0-fc83-43bf-a524-d86648ddbf27" />

3. Launch Container In that Network:


       docker run -dit --name database \
       --network psnet -v /mydata:/var/lib/mysql \
       -e MYSQL_ROOT_PASSWORD=pratik55  \
       -e MYSQL_DATABASE=mydatabase
       -e MYSQL_USER=jack
       -e MYSQL_PASSWORD=jack11 mysql


-----

-----

# Site-to-Site VPN at Mumbai Region: CREATE IPsec VPN [Dummy]
For AWS-to-AWS Site-to-Site VPN, I created Customer Gateways with temporary public IPs and then updated them using AWS-assigned VPN tunnel outside IPs.

1. STEP-1: Create Virtual Private Gateway (VGW)
   
Create VGW this is nedpoint of out VPC of mumbai region.

Steps:
  - VPC Console → Virtual Private Gateways-> Create VGW (Name: vgw-mumbai)
  - ASN: 64512
  - Attach to Mumbai VPC 
<img width="1593" height="277" alt="image" src="https://github.com/user-attachments/assets/0b77e2bf-28ec-4125-bf10-cfba1594cc7d" />

Same time created at sydnet region.

2. STEP 2: Create Customer Gateways (CGW)

Mumbai: Create Customer Gateway (for Sydney)

Here both region we use AWS VPN so we need 1st make dummy IP for other region customer gateway :

Here i used 8.8.8.8 public ip for test 1st time.

<img width="1523" height="427" alt="image" src="https://github.com/user-attachments/assets/a6262e17-769c-44d0-881c-339a84ae17a1" />



3. Site-to-Site VPN Connections

Target Gateway: Virtual Private Gateway

Virtual Private Gateway: vgw-mumbai

Customer Gateway: Existing

Customer Gateway: cgw-sydney-temp

Routing: Static
Static Route: 10.20.0.0/16

This is Testing we still not put sydney region info:
<img width="1577" height="707" alt="image" src="https://github.com/user-attachments/assets/26baadbf-26ba-4587-9862-3cc8ffeb902b" />


- AWS has assigned Outside IPs:

  Tunnel 1: 13.201.111.175
  Tunnel 2: 43.204.9.161


-----


# sydney region site-to-site VPN: [Dummy]
For AWS-to-AWS Site-to-Site VPN, I created Customer Gateways with temporary public IPs and then updated them using AWS-assigned VPN tunnel outside IPs.

1. STEP-1: Create Virtual Private Gateway (VGW)
<img width="1555" height="251" alt="image" src="https://github.com/user-attachments/assets/d7f3d042-277c-49f0-b7b6-c87f620f74b2" />


2. STEP 2: Create Customer Gateways (CGW)

Here both region we use AWS VPN so we need 1st make dummy IP for other region customer gateway :

Here i used 8.8.4.4 public ip for test 1st time.
<img width="1515" height="260" alt="image" src="https://github.com/user-attachments/assets/2a6345f2-8924-408f-b79e-46a06a250406" />

3. Site-to-Site VPN Connections:

<img width="1508" height="661" alt="image" src="https://github.com/user-attachments/assets/a6943eab-9de7-4251-bda9-ef26da72105d" />


-----



