# Designing-and-Securing-a-Cloud-Infrastructure-using-AWS

## Objective

The aim of this project is to design and secure a cloud Infrastructure using Amazon Web Services that is capable of hosting Websites or web applications. Enhance security and compliance on the infrastructure and capabable for high availability and resilience of the infrastructure. This hands-on experience was designed to deepen understanding of cloud and security services offered by AWS.


## Skills Learned
[Bullet Points - Remove this afterwards]

Advanced understanding of SIEM concepts and practical application.
Proficiency in analyzing and interpreting network logs.
Ability to generate and recognize attack signatures and patterns.
Enhanced knowledge of network protocols and security vulnerabilities.
Development of critical thinking and problem-solving skills in cybersecurity.

## Service Used

- Virtual Private Network (VPC)
- Elastic Compute Cloud (EC2)
- Relational Database Service (RDS)
- Route 53
- Auto Scaling
- Elastic load balancing (ELB)
- Internet Gateway (IGW)
- Identity and Access Management (IAM)
- Multi-Factor Authenticator (MFA)
- Web Application Firewall (WAF)
- AWS Certificate Manager (ACM)  
- Multi Availability Zone

## Step 1: Cloud Architecture
Cloud Architecture

 <img width="935" height="362" alt="AWS ARCHITECTURE 3" src="https://github.com/user-attachments/assets/121a9804-8b59-43e0-b5af-8b2ae2c30c0a" />
 
Figure 1. AWS Cloud Architecture

Figure 1 shows the AWS Cloud architecture design to be implemented. The architecture has a Virtual Private Cloud which will allow the resources to be launched within it. The core service being an EC2 (Webserver) and RDS MySQL Database. The architecture consists of 2 Availability Zone, 4 Subnets (Public & Private), Security groups (for the webserver and database) and the services like Internet Gateway, Route 53, Elastic load Balancing and EC2 Auto Scaling. The security features are IAM, MFA, Web Application Firewall, SSL/TLS Certificate and Network Access control list.   

### Region 

In Figure 2, Stockholm (eu-north-1) is the region selected because it is close to the location where the infrastructure is intended for use. This also results in lower costs for the use of services

<img width="151" height="130" alt="region2" src="https://github.com/user-attachments/assets/34152aa9-4255-46f8-9fba-6699eb7b72d4" />

Figure 2.Region

## Step 2:  Setting up AWS Virtual Network

In Figure 3, a VPC is created to serve as a logical virtual network that makes it possible to launch other AWS resources within it. The IPv4 Classless Inter Domain Routing (CIDR) network block for this VPC is 10.0.0.0/16. This subset can be used to construct subnets.

 <img width="557" height="279" alt="Create VPC" src="https://github.com/user-attachments/assets/0748234e-6479-43a8-a05d-b3b35aa93201" />
 
Figure 3. Create VPC


### Create Subnets (Public & Private) 

In Figure 4, Two subnets are created within the VPC. Public Subnet named PublicSubnet1-spidertech and Private subnet named PrivateSubnet1-Spidertech. Public subnets interact with the outside network of the infrastructure while the private subnet only interacts within the infrastructure e.g. between the webserver and database. The availability zone selected is Europe (Stockholm)/eun1-az1(eu-north-1a). PublicSubnet1-spidertech is allocated the Ip address 10.0.0.0/24. 

<img width="653" height="439" alt="PublicSubnet1-ST" src="https://github.com/user-attachments/assets/9ff8244a-647a-465a-bc02-d464900825ec" />

Figure 4.Subnet Settings: Public Subnet


In Figure 5, the same configuration as figure 4 are configure but for the Private subnet named PrivateSubnet1-SpiderTech and is allocated the Ip address 10.0.1.0/24

 <img width="434" height="449" alt="PrivateSubnet1-ST" src="https://github.com/user-attachments/assets/8af99821-64c9-4d6a-b5fe-94e4195f07ef" />

Figure 5.Subnet Settings: Private Subnet

### Setup Internet Gateway (IGW) 

In Figure 6, the internet gateway named IGW-SpiderTech is created and allows internet access

 <img width="765" height="258" alt="Create IGW" src="https://github.com/user-attachments/assets/c7e0daff-429f-4328-b2f3-29caf0c8d6c1" />
 
Figure 6: Create Internet Gateway


### Attach VPC to Internet Gateway 

In Figure 7, the VPC is attached to the IGW to allow communicate with internet

 <img width="756" height="191" alt="attach VPC to IGW" src="https://github.com/user-attachments/assets/22d041d5-e523-47e6-b0e7-5b5f997ae12c" />

Figure 7. Attach IGW to VPC

### Create Route Tables (Public & Private) 

In Figure 8, Route tables specify how packets are forwarded between subnets within the VPC and internet. Public- RT-Spidertech name tag is created for the public route table and attached to the VPC-Spidertech.

<img width="886" height="298" alt="Route Table-Public" src="https://github.com/user-attachments/assets/80edbada-1cc2-4ab3-ba02-4aeb772ceca6" />
 
Figure 8. Create Route Table

### Edit Public Route Table 

In Figure 9, Ipv4 0.0.0.0/0 is selected as the destination allowing all addresses to be forwarded to the targeted Internet Gateway (IGW-SpiderTech)

<img width="868" height="193" alt="edit route table- public" src="https://github.com/user-attachments/assets/207ed66f-f193-4aea-ba90-c86e2a20a5b7" />

 
Figure 9. Edit Public Route Table


### Attach Public Subnet to Public Route Table 

In Figure 10, the route table specifies the route of how the public subnet packets are forward by association with the public route table

<img width="868" height="233" alt="attach public subnet to public route" src="https://github.com/user-attachments/assets/88b5df21-e4d6-4519-ac1a-7a707b9a9ce7" />
 
Figure 10. Attach Public Subnet to Public Route Table

### Attach Private Subnet to Private Route Table 

In Figure 11, the route table specifies the route of how the private subnet packets are forward by association with the private route table

 <img width="868" height="239" alt="attach private subnet to private route" src="https://github.com/user-attachments/assets/647488ae-27bf-4808-9b11-28e496df1717" />

Figure 11. Attach Private Subnet to Private Route Table


### Create Security Groups (Webserver) 

In figure 12, webserver security group is created to control inbound and outbound traffic by adding rules for which Protocols/Ports are given access or not. The security group Webserver-SG allow http & SSH Protocol connection and is attached to VPC-Spidertech. Inbound rules are then configured to only allow Http & SSH traffic into the EC2 webserver from any destination and all IP address

<img width="578" height="419" alt="webserver SG" src="https://github.com/user-attachments/assets/2d013371-812b-47de-8fad-4bd4c7f86ccf" />

Figure 12. Create Security Groups

### Outbound rule 

In figure 13, the outbound rules allow all traffic, Protocols and port numbers from webserver to move out to all different destinations and IP addresses.

<img width="975" height="171" alt="image" src="https://github.com/user-attachments/assets/51682544-050e-43cc-ae90-4200182b953a" />

Figure 13. Outbound Rules

## Step 3 : Launching and Configuring Elastic Cloud Compute (EC2) 

In figure 14, virtual machine named spidertech-webserver is launched installed with an Operating System (Amazon Linux) running on a 2023 kernel 6-12 Amazon Machine Image, instance type t3.micro, self-generated keypair for security access, attached to the Spidertech-VPC and Webserver Security Group. 8 Gb storage with 3gp root volume

<img width="876" height="595" alt="image" src="https://github.com/user-attachments/assets/9e98cecd-e61a-4b6c-9769-f7006472a140" />
 
Figure 14. Launch an Instance

In figure 15, I selected a Free tier (not chargeable) instance type, spidertech-keypair to be able to connect to the instance securely via ssh or console. Selected VPC-spidertech for the network, enable auto-assign public IP that will be given to the instance when its actively running.

 <img width="583" height="425" alt="ec2-2" src="https://github.com/user-attachments/assets/b7a8b442-0b6e-4a16-b092-2c777e7658ba" />

Figure 15. Launch Instance: Instance Type, Key Pair & Network Settings

In figure 16, Select existing security group and choose Webserver-SG. The storage capacity is set at 8 GB running on a gp3 root volume. After that the instance is launched.

<img width="861" height="606" alt="image" src="https://github.com/user-attachments/assets/a8e71f7a-65ad-4bc4-88d6-ae608962b909" />

 
Figure 16. Launch Instance: Security Group & Configure Storage

### Connect to EC2

In figure 17, Tick on Webserver-Spidertech, under actions click “connect to ec2 instance”. Select connect type “Connect using a Public IP” and public IPV4 address which is automatically assigned then click connect.

 <img width="975" height="365" alt="image" src="https://github.com/user-attachments/assets/114592d4-888a-4061-a686-cbaf2fb109d1" />

Figure 17. Connect EC2

In figure 18, The Linux Operating System is running successfully

 <img width="778" height="385" alt="image" src="https://github.com/user-attachments/assets/ef8b1cb8-75b5-40a8-b11e-62500b1aad2b" />

Figure 18. EC2 Running Linux OS


## Step 4 : Installing & Configuring WordPress

#### a)	Update and upgrade packages 
Command: Sudo yum update (Figure 19)

<img width="680" height="239" alt="image" src="https://github.com/user-attachments/assets/89a9d56b-f90f-4292-aa97-8f10478f5f25" />
 
Figure 19. Update & Upgrade Linux Packages

#### b)	install Apache Web server 
Command: Sudo yum install httpd -y (Figure 20)

 <img width="1031" height="753" alt="image" src="https://github.com/user-attachments/assets/b9b05bea-4e12-4eaa-8436-851babae64ec" />

Figure 20. Apache Installation

#### c)	Install PHP and MySQL 

Command: Sudo yum install -y MySQL & Sudo dnf install-y httpd mariadb105-server php php-mysqlnd (Figure 21)

<img width="969" height="201" alt="image" src="https://github.com/user-attachments/assets/f13096a1-64b8-4b59-bcd2-1f4de7e811f8" />
 
Figure 21. PHP & MySQL Installation

#### d)WordPress Setup 
Command: Sudo wget https://wordpress.org/lastest.tar.gz (Figure 22)

 <img width="832" height="404" alt="image" src="https://github.com/user-attachments/assets/971f5891-ae5b-4e85-8288-44d18358c7f6" />

Figure 22. Verifying PHP & MySQL Installation

#### e)	Create WordPress Database on EC2 Webserver
Command: export MYSQL_HOST=database-2.cpaiqgcwoe2t.eu-central-1.rds.amazonaws.com (Figure 23)

 <img width="954" height="339" alt="image" src="https://github.com/user-attachments/assets/d9a79ff9-2f26-4b2c-b98c-ffe3096b9d52" />

Figure 23. Export MySQL Database to EC2



### Second set of Subnets

In Figure 24, Two extra subnets are created within the VPC. Public Subnet named PublicSubnet2-spidertech and Private subnet named PrivateSubnet2-Spidertech.The availability zone selected is Europe (Stockholm)/eun1-az1(eu-north-1b). PublicSubnet1-spidertech is allocated the Ip address 10.0.2.0/24. PrivateSubnet1-spidertech is allocated the ip address 10.0.3.0/24. PrivateSubnet2 is created in order to make avail a second Availability zone which is required for configuring an RDS Database.

<img width="864" height="680" alt="image" src="https://github.com/user-attachments/assets/7fcf8091-4bb5-42c4-a39a-81b760edc8a9" />

Figure 24. Create Private Subnet


In Figure 25, Attach PrivateSubnet2-Spidertech subnet to the Private Route table

 <img width="869" height="326" alt="image" src="https://github.com/user-attachments/assets/f229aaba-7e46-4b86-981f-80c542f7e5b1" />

Figure 25. Attach Private Subnet to Private Route Table

## Step 5 :  Create RDS Database subnet Group 

In Figure 26, The name tag created is Spidertech-DB-SubnetGroup and this is to control the inbound and out bound traffic to and from the RDS database. Selected VPC-Spidertech and Attached to 2 availability zone (eu-north-1a and eu-north-1b) and both private subnets (PrivateSubnet1-Spidertech and PrivateSubnet2-Spidertech) because it’s a requirement for creating a database security group

 <img width="895" height="616" alt="image" src="https://github.com/user-attachments/assets/e15f99e6-26f0-47ca-bb1e-96c959b7689b" />

Figure 26. Create RDS Database Subnet Group


### Create RDS Database as a MySQL engine

In Figure 27, Creating method used to create the database is Standard with a MySQL engine type.

 <img width="975" height="457" alt="image" src="https://github.com/user-attachments/assets/6e1a5d33-0c36-4622-9811-a12fd0bffcb3" />

Figure 27. Create RDS Database


### Create Security Groups (RDS Database) 

In figure 28, the security group control inbound and outbound traffic by adding rules for which Protocols/Ports are given access or not. The security group RDS Database is created to   allow http & SSH Protocol connection and attached to VPC-Spidertech. Inbound rule allows only the web-security group to access database-security group for MySQL protocol/Type

<img width="851" height="586" alt="image" src="https://github.com/user-attachments/assets/72d10b1e-19da-4e0c-b9da-627441cbab44" />
 
Figure 28. Create DB Subnet Group

In Figure 29, Inbound rule for the DB security group allows only the web-security group to access database-security group for MySQL protocol/Type

 <img width="819" height="217" alt="image" src="https://github.com/user-attachments/assets/66e8e94c-dcae-471c-a4de-fe77d54d8a79" />

Figure 29. Edit RDS Inbound Rules

### Configure the WordPress Database on EC2

 In Figure 30, Define DB name, Db user, Db password and DB host information
 
 <img width="724" height="524" alt="image" src="https://github.com/user-attachments/assets/44983302-53f9-4e68-b233-d7cc933fd365" />

Figure 30. Configure WordPress Database

In Figure 31, after configuring on the EC2 database, paste   http://(EC2_Public_IP_Address)/wp-admin with the public IP address of the Ec2 instance to start the WordPress installation.

<img width="716" height="708" alt="image" src="https://github.com/user-attachments/assets/da2d1ea3-0439-494e-9df1-7b7365e724c4" />

Figure 31. WordPress Installation

In Figure 32, WordPress Web application running successfully

 <img width="821" height="472" alt="image" src="https://github.com/user-attachments/assets/79bbf0e9-324c-4e5f-ac0d-5a5eaee4894e" />

Figure 32. Running Web Application

## Step 6:  Create Second EC2 for redundancy 

In Figure 33, Stop EC2 instance, select Actions, click instance settings and click “create image”

 <img width="975" height="213" alt="image" src="https://github.com/user-attachments/assets/3af13d27-9fc6-47aa-8be7-43e03f673860" />

Figure 33. Create Image for AMI

In Figure 34, Select Amazon Machine Imagine (AMI) from search bar and name tag “Spidertech-web-app” then launch AMI

<img width="975" height="185" alt="image" src="https://github.com/user-attachments/assets/85dd1340-3c68-4bd1-a5d2-b5e99c8210cc" />
 
Figure 34. Launching an AMI

### Create a Launch Template 

In Figure 34, This template create/replicates a saved Ec2 instance that can be launched and reused. Without the need to configure another EC2 instance with the same configurations. Tag name “Spidertech-Web-App-AMI” with a description 

 <img width="903" height="504" alt="image" src="https://github.com/user-attachments/assets/7bdad96d-376e-4f62-a44e-1adfafcab8d5" />

Figure 35. Create Launch Template

In Figure 36, Select “My AMIs” and “Owned by me”. The select Spidertech-Web-App as the AM1

 <img width="849" height="636" alt="image" src="https://github.com/user-attachments/assets/a9b866da-549b-4a41-9c2f-17fe5af99f2c" />

Figure 36. Create a Launch Template: AMI.

In Figure 37, Select a t3.micro instance type, spidertech-keypair to be able to connect to the instance securely via ssh or console. Select Webserver-SG as the existing group.

 <img width="972" height="629" alt="image" src="https://github.com/user-attachments/assets/823dec61-005e-4784-bc27-9e9cf1ddef9f" />

Figure 37. Create a Launch Template: Instance type, Key Pair, Network Settings


In Figure 38, Under user data the commands are enter into the field in order for the template to automatically start when required

- #!/bin/bash
- Yum update -y
- Sudo service httpd restart

  

 <img width="976" height="740" alt="image" src="https://github.com/user-attachments/assets/fcd736f9-6c7e-4841-bb06-676db8d11377" />

Figure 38.Create a Launch Template: Bash Command



### Create Auto-Scaling Group

Launch template: Name tag is Spidertech-ASG and set Default version 1 then Next (Figure 39)

<img width="926" height="506" alt="image" src="https://github.com/user-attachments/assets/fcb9e769-826e-42b4-a6c8-146082c0b2b3" />

Figure 39. Create Auto-Scaling Group


Launch Options: Choose VPC-Spidertech and select both Public Subnets with Availability Zones eu-north-1a & 1b. The distributions of the Availability Zone set to “Balanced best effort” for automatic balance on the instances. (Figure 40)
 
 <img width="863" height="585" alt="image" src="https://github.com/user-attachments/assets/51c5ad05-58bf-49bb-9c9a-7465dfa7c179" />

Figure 40. Create Auto-Scaling Group: Instance Launch Option


 No load balancer because it has not been created yet (Figure 41)
 
 <img width="788" height="540" alt="image" src="https://github.com/user-attachments/assets/51f10811-5c8a-4aad-aaf4-06e44d62ac82" />
 
Figure 41. Create Auto-Scaling Group: Load Balancing


In Figure 42, Desired capacity for instance always running idle is set to 2 for the group size. Under Scaling, Min desired capacity for increment is set to 1 and maximum instance to run is 4 

 <img width="854" height="587" alt="image" src="https://github.com/user-attachments/assets/ec3ca8aa-893e-4c01-9787-0f2c5d44f34b" />

Figure 42. Create Auto-Scaling Group: Configure Group Size & Scaling


In Figure 43, adding a notification via email depending on the event type (optional)

 <img width="975" height="318" alt="image" src="https://github.com/user-attachments/assets/47271d01-dfda-4ed1-88da-a1948d70a740" />

Figure 43. Create Auto-Scaling Group: Add Notification


In Figure 44, shows 2 successful instances running 

<img width="978" height="177" alt="image" src="https://github.com/user-attachments/assets/8e322998-cc04-4bfc-a5bd-5f08b65dc3d6" />
 
Figure 44. Successful Auto Scaling Activity


### Create Target Group: Loading Balancing

In Figure 45, First Create Target group then select target type for the resource, which is “instance”. Application Load Balancing is most suited for the instance. This helps by launching another EC2 instance hence horizontally scaling out in case the main webserver is over loaded with requests and making it resilient. Group name entered “Spidertech-TargetGroup-WebApp” with the HTTP protocol and port 80 selected.

 <img width="795" height="617" alt="image" src="https://github.com/user-attachments/assets/34e5f788-6a9d-4044-b40a-71130a1e6443" />

Figure 45. Create Target Group for Load Balancing


In Figure 46, Select IPv4 as the address type, VPC-Spidertech and HTTP1 as the protocol version.

<img width="895" height="424" alt="image" src="https://github.com/user-attachments/assets/275e457d-c8c7-4a76-bde9-25263a1f5c05" />

Figure 46. Create Target Group: IP address Type, VPC & Protocol Version


 <img width="975" height="506" alt="image" src="https://github.com/user-attachments/assets/552f4e9c-eb5a-4b2c-ac6c-3d9ce49efd5d" />

Figure 47. Create Target Group: Register Targets


### Load Balancer
In Figure 48, Name is set to Spidertech-LB and the scheme selected is “Internet- Facing” with IPv4 as the address type.

 <img width="975" height="431" alt="image" src="https://github.com/user-attachments/assets/c9bbd507-c6f0-4f22-ba58-f591f666be40" />

Figure 48. Create Load Balancer


In Figure 49, VPC-Spidertech is selected for the VPC. Tick both Availability Zones eu-north-1a and eu-north-1b with the corresponding Public Subnets (PublicSubnet1-Spidertech & PublicSubnet2-Spidertech) 

 <img width="975" height="441" alt="image" src="https://github.com/user-attachments/assets/3481e62c-ff62-4795-b531-03beead432cb" />

Figure 49. Create Load Balancer: Network Mapping


In Figure 50, shows registered target associated with Webserver-SG

 <img width="878" height="455" alt="image" src="https://github.com/user-attachments/assets/c1040e39-af90-420a-95f6-879388330325" />

Figure 50. Create Load Balancer: Register Targets





### Create a RDS Database read replica

In Figure 51, Click “Actions” and select “Create replica”

<img width="940" height="484" alt="image" src="https://github.com/user-attachments/assets/57cd0da6-0287-485e-83b6-865f1bd1e623" />

Figure 51. Create RDS Database Read Replica


In Figure 52, the read replica database is created with the same attributes as the main database

<img width="940" height="264" alt="image" src="https://github.com/user-attachments/assets/8da87143-d19d-484e-bafa-e54b329a1c66" />
 
Figure 52. Read Replica Running



## Step 7 : Security

### Network Access Control List (NACL)

In Figure 53, Two inbound rules are configured. Rule 100 allows all traffic flow into the Network. Rule * apply to the rest of the rule number that deny traffic into the network. ACL operates on a priority fashion which give the lowest Rule number preference over the next rule number  

 <img width="940" height="246" alt="image" src="https://github.com/user-attachments/assets/fc9ab41a-1b74-4ada-8678-f73940e6fcb9" />

Figure 53. NACL Inbound Rules


In Figure 54, Outbound rule allows traffic within the network to flow outwards.
 
 <img width="940" height="249" alt="image" src="https://github.com/user-attachments/assets/8fc22879-3fda-44d3-8cec-07b43bd07d4c" />

Figure 54.NACL Outbound Rules


### Web Application Firewall (WAF)

In Figure 55, Region Scope is set to Regional 

 <img width="940" height="368" alt="image" src="https://github.com/user-attachments/assets/4a8fcc2c-a43a-4404-b3d4-a86904de0549" />

Figure 55. Choose Regional Scope for Protection Scope


In Figure 56, App focus is set to Web for the creating a web ACL protection pack

 <img width="940" height="425" alt="image" src="https://github.com/user-attachments/assets/bd18850e-2f5c-4de2-abe2-6976db85926d" />

Figure 56. Create Protection Pack (Web ACL)






In Figure 57, The resource to be protected is set to Spidertech-Load Balancer.

<img width="940" height="408" alt="image" src="https://github.com/user-attachments/assets/55a360d0-eeba-4c0e-9721-4d7f18603a0b" />
 
Figure 57. Add Resource


In Figure 58, Set rule to inspect “all request”

 <img width="940" height="549" alt="image" src="https://github.com/user-attachments/assets/8c8cba33-d89f-453f-a06b-bebad2c6afbd" />

Figure 58. Add Rules


In Figure 59, Rule group contains Rule provided by AWS for specifically WordPress applications and are set to “Block” the exploits paths and commands

<img width="828" height="364" alt="image" src="https://github.com/user-attachments/assets/e11582f5-7ef3-41ef-ada5-9b4395289232" />

Figure 59. Rules Group


In Figure 60, Rules are shown in Priority Order

 <img width="923" height="498" alt="image" src="https://github.com/user-attachments/assets/6e990a26-9fd5-4e4d-a157-bca5c8ea9446" />

Figure 60. Rules in Priority Order


### AWS Certificate Manager

In Figure 61, An SSL/TLS certificate is requested since we don’t own one

 <img width="940" height="255" alt="image" src="https://github.com/user-attachments/assets/bc7aadc5-9127-41e9-be71-3500430a6d46" />

Figure 61. Request Certificate


In Figure 62, Provide a Domain name “PublicCertificate.spidertechologies.com” and set DNS validation as the method

 <img width="865" height="717" alt="image" src="https://github.com/user-attachments/assets/3e0ef4a0-b5c1-4c0b-9eb4-22747775fe73" />

Figure 62. Request Public Certificate


In Figure 63, Pending Validation for the requested Certificate

 <img width="940" height="276" alt="image" src="https://github.com/user-attachments/assets/379f4956-d0ab-4b4e-871e-68907d5d11e2" />

Figure 63. Certificate Pending Validation


### Identity and Access Management (IAM) and Multi-Factor Authentication

In Figure 64, the IAM user is being created, the user name “user1” and password are being set

 <img width="940" height="454" alt="image" src="https://github.com/user-attachments/assets/9fb4ee40-dab0-456b-83ef-11374aef7d62" />

Figure 64. Create IAM User




In Figure 65, shows the permission options are set to “attach policies directly” then the permission policies for the user are selected

 <img width="940" height="357" alt="image" src="https://github.com/user-attachments/assets/a49f87f3-1f56-4f4b-bc61-44e7080dccc4" />

Figure 65: IAM: Set Permissions



In Figure 66, shows the permission policies that have being selected for the user. 

 <img width="940" height="348" alt="image" src="https://github.com/user-attachments/assets/8a0154ff-a6aa-4d04-be1b-663b0dc27b49" />

Figure 66. IAM: Review Permissions




In Figure 67, shows the user credentials consisting of their console sign-in URL, username and console password.

<img width="940" height="382" alt="image" src="https://github.com/user-attachments/assets/f715f33b-247f-49e3-ba85-50c651f21a7a" />
 
Figure 67. IAM: Retrieve Credentials

In Figure 68, shows google authenticator, an application that generates a passcode for multi-factor authentication when the credentials in figure 67 are used to sign in.

 <img width="421" height="524" alt="image" src="https://github.com/user-attachments/assets/bed26ef4-9682-4511-989a-11725e02d63d" />

Figure 68. Multi-Factor Authentication




