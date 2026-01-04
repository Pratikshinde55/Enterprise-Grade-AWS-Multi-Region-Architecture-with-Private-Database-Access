# DevOps Portfolio Platform on AWS using ALB, Private VPC, 
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
       -e MYSQL_DATABASE=mydatabase \
       -e MYSQL_USER=jack \
       -e MYSQL_PASSWORD=jack11 \
       -p 3306:3306 \
       mysql


5. Login to Container using root: (pass- pratik55)

      docker exec -it database mysql -u root -p

6. Fix the user properly:

      USE mysql;

7. Drop user if partially exists:

     DROP USER IF EXISTS 'jack'@'%';
     DROP USER IF EXISTS 'jack'@'localhost';

 8. Create user (allow remote access):

     CREATE USER 'jack'@'%' IDENTIFIED BY 'jack11';

9. Grant access to your DB:

      GRANT ALL PRIVILEGES ON mydatabase.* TO 'jack'@'%';
      FLUSH PRIVILEGES;

10. Verify user:

      SELECT Host, User FROM mysql.user WHERE User='jack';

11. Test From Local Host:(Pass jack11)

      docker exec -it database mysql -u jack -p

-----

-----

# VPC Peering Different regional: [AT Mumbai region]
From Mumbai region we sent req vpc peering to the Sydney region

## 1. Create VPC Peering Request (Mumbai):

Go to VPC → Peering Connections -> Click Create peering connection

VPC (Requester) -> PS-Workpress-vpc-mumbai [This is our mumbai region VPC name]

Account -> 	My account

Region	-> Another region

Region	-> ap-southeast-2 (Sydney)

VPC ID (Accepter)-> 	vpc-09399cfe5356f7616 [sydney region vpc PS-DataBase-VPC id]

<img width="1705" height="695" alt="image" src="https://github.com/user-attachments/assets/3c92a354-acdb-42cd-9ab7-8b7de13f9aab" />


Here we can see vpc peering req send from mumbai region to sydney region:
<img width="1677" height="610" alt="image" src="https://github.com/user-attachments/assets/ad98dc97-c7d8-42b5-bb92-aba760b881dd" />

## 2. Accept request at sydney region: (Sydney region)
Sydney region we can see at VPC Perring Connection:
<img width="1473" height="277" alt="image" src="https://github.com/user-attachments/assets/d64cc5c3-b67e-4629-a857-8bfe9f699bd5" />

Go To "Action" and press "Accept":
<img width="992" height="382" alt="image" src="https://github.com/user-attachments/assets/78c6576e-aa51-4da0-9d5f-f6fe6dcd6119" />

Now We can see Status is Active On Region -> Peering Connection:

Sydney Region: (Peering Connection)
<img width="1496" height="350" alt="image" src="https://github.com/user-attachments/assets/8c4b4674-4cd7-47d8-ac90-81eaee6f7591" />


Mumbai Region: (Peering Connection)
<img width="1501" height="345" alt="image" src="https://github.com/user-attachments/assets/73b095fa-317d-4339-bff8-4ab9d4efee26" />



## 3. Allow Private Route Table To Pass VPC Flow at both region: [NAT Route table is Private route table]

Mumbai region NAT route Table:(Mumbai private route table)

Destination: 10.10.0.0/16   

Target: Peering connection

<img width="1426" height="307" alt="image" src="https://github.com/user-attachments/assets/51d81345-f4f3-41e4-bbae-7b2d82dafd29" />


Sydney region NAT route Table: (Sydney private route table)

Destination: 10.0.0.0/16
  
Target: Peering connection

<img width="1521" height="307" alt="image" src="https://github.com/user-attachments/assets/ff47e875-345c-49a8-afe0-b6d95d77f3a4" />

## Now on Both Region Ec2 allow security group- inbound rule:

Mumbai EC2 inbound rule: (Source 10.10.0.0/16 is sydney vpc mask)
<img width="1407" height="412" alt="image" src="https://github.com/user-attachments/assets/374ad029-3aab-43da-8cfe-d6c8da7f449b" />

Sydney EC2 inbound rule: (Source 10.0.0.0/16 is Mumbai VPC Mask) 
<img width="1393" height="497" alt="image" src="https://github.com/user-attachments/assets/527a320e-e3b3-47bc-9bc7-9f76b434b02b" />


## Test From Both EC2 to other region EC2 Private IP: (Private IP Connection work)

Mumbai Region EC2 To Sydney region EC2 Private IP ping:
<img width="1486" height="182" alt="image" src="https://github.com/user-attachments/assets/b40731d7-f551-4946-a742-51ba443c04a9" />

Sydney Region EC2 To Mumbai region EC2 Private IP ping:
<img width="1572" height="193" alt="image" src="https://github.com/user-attachments/assets/07529f92-b117-4530-8387-c973d55220e0" />


-----

-----

# Mumbai EC2 Workpress install:

install workpress:

      docker run -dit \
      --name wordpress \
      -p 8080:80 \
      -e WORDPRESS_DB_HOST=10.10.2.145:3306 \
      -e WORDPRESS_DB_NAME=mydatabase \
      -e WORDPRESS_DB_USER=jack \
      -e WORDPRESS_DB_PASSWORD=jack11 \
      wordpress

-----

-----

#  ALB Set-up Mumbai Region:

## 1. Create Target group: (Only one we create)
 
Target type	 --> Instance

Protocol	--> HTTP

Port	--> 80 or 8080

IP address type	--> IPv4

VPC	--> PS-Workpress-vpc-mumbai

<img width="1790" height="839" alt="image" src="https://github.com/user-attachments/assets/04dccf13-0a41-4da1-bc31-1b9799649846" />

Success code 200-399 that make alive site because we use Login for Admin:
<img width="1742" height="839" alt="image" src="https://github.com/user-attachments/assets/99bd69d3-709d-497e-8ed5-7b3b8f49c90a" />

Include EC2 with the workpress container exposed 8080:
<img width="1881" height="720" alt="image" src="https://github.com/user-attachments/assets/a7d88488-13a6-4479-9664-7c7cabf7129a" />



## 2. Create ALB Security Group (PUBLIC ENTRY)
EC2 --> Security Groups --> Create security group

USE same VPC of Mumbai region: VPC	--> PS-Workpress-vpc-mumbai

<img width="1547" height="583" alt="image" src="https://github.com/user-attachments/assets/258c6612-e980-4eff-b748-7b61d84c9c82" />



## 3.  Edit Security Group of Mumbai EC2 workdprees: (Allow alb-sg to come in ec2)

Note: We use old Security group of EC2 and just add new inbound rule that allow traffic from alb-security group at port no 8080.

We can also able create seperate new Security group & add the ec2.

Old Ec2 Mumbai Region Security group --> name:Workpress-EC2-SG
<img width="1517" height="386" alt="image" src="https://github.com/user-attachments/assets/ffc40d2b-7deb-43ba-b0b0-c35c79aa0981" />


## 4. Create Application Load Balancer:
EC2 -->  Load Balancers --> Create Load Balancer

Basic confifuration of ALb:
<img width="1776" height="618" alt="image" src="https://github.com/user-attachments/assets/8b12bd25-06b6-4e64-bdad-987021e74be4" />


Network Mapping: (Must Launch in Public Subnet Then Only Work)
<img width="1810" height="652" alt="image" src="https://github.com/user-attachments/assets/f375b053-1c02-4090-9ea6-2f4a2ab94317" />

Security group: Select security group that we created for ALB
<img width="1313" height="188" alt="image" src="https://github.com/user-attachments/assets/aba75146-acc7-4ba9-ac00-07863076e83f" />

Listeners and routing: (When we HTTPS SSL then we use HTTPS otherwise use HTTP-80)
<img width="1585" height="623" alt="image" src="https://github.com/user-attachments/assets/ce0e3ef5-444d-439a-8c64-3d7ffab3ce51" />

## 5. Now Set Path Lisener at ALB:

Add /* rule that allow all wordpress api call to come in ec2:
<img width="1452" height="538" alt="image" src="https://github.com/user-attachments/assets/4b4b034e-140f-47ac-979d-b574c0f88063" />

Then Action:
<img width="1443" height="542" alt="image" src="https://github.com/user-attachments/assets/ec8caf1c-0fce-4c56-b682-8cdb43207d68" />



Note:

- In Target Group The Health status maust be **"Healthy"**, That means the target group heat the 8080 prot health check path and that available.
- At ALB Listeners and rules -> we create HTTP because i don't have registered  Domain, If domain name have then use HTTPS with ALB SLL certificate manage by ACM.
- In HTTP listener i add "Rule" that is Path routing.
- Workflow:  ALB --> Listener(HTTP or HTTPS)  --> Rule (Path routing: /admin or /welcome) --> Target Group(Here mention ec2-8080)  --> EC2 at 8080 workpress app.
- ALB Rule: Priority [Added other Path that needed for wordpress]
 <img width="1533" height="423" alt="image" src="https://github.com/user-attachments/assets/f89a8ddb-e7e1-4428-aeae-acacfe06ab27" />



Now APP is live we use alb DNS name  to access site: (alb-wordpress-155777051.ap-south-1.elb.amazonaws.com)

Here just /welcome /admin add at last path.

1st time admin add use /wp-login.php

-----

-----

# Access App from Browser:

## 1. 1st time access app then need login needed:
http://alb-wordpress-155777051.ap-south-1.elb.amazonaws.com/wp-login.php

Here Create Admin: (wp-login.php this wordpress official login route)

<img width="1592" height="921" alt="image" src="https://github.com/user-attachments/assets/152e951c-7991-447d-af86-4c2a17571fe4" />


## Create user page come and install wordpress:
<img width="1526" height="925" alt="image" src="https://github.com/user-attachments/assets/1c8b4a98-fa9d-4f57-bff9-5ef586b8cf35" />

## 
