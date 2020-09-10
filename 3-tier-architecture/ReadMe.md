# STEP1: Create VPC and EC2:
  ## 1.Create VPC:
    VPC:
        Name: Learn 3Tier Architecture
        IPv4 CIDR Block: 10.10.0.0/16
    Subnets:
        Public subnet 1a (Az 1-a):
            10.10.1.0/24
        Public subnet 1b (Az 1-b):
            10.10.2.0/24
        Private subnet 1a (Az 1-a):
            10.10.3.0/24
        Private subnet 1b (Az 1-b):
            10.10.4.0/24
    IGW:
        Name: Learn 3Tier Architecture
        Attach it to the "Learn 3Tier Architecture" VPC
    Route Tables:
        Private Route Table
            Routes:
                10.10.0.0/16    local
                0.0.0.0/0       the "Learn 3Tier Architecture" NAT gateway
            Subnet Assocication:
                Private subnet 1a
                Private subnet 1b
        Public Route Table
            Routes:
                10.10.0.0/16    local
                0.0.0.0/0       the "Learn 3Tier Architecture" IGW gateway
    NAT (to let private subnet can access internet):
        Name: Learn 3Tier Architecture
        Subnet: Public subnet 1a
        Auto asign an Elastic IP:
            TagKey: Name
            TagValue: Learn 3Tier Architecture
    Security Groups:
        Wordpress-SG:
            inbounds: ssh, http, https
        RDS-SG:
            inbouds: Mysql/Aurora  -> Wordpress-SG

        
  ## 2.Create EC2:  
        bootstrap:
            #! /bin/bash
            yes | sudo apt update
            yes | sudo apt install wordpress php libapache2-mod-php mysql-server php-mysql
            yes | sudo chkconfig httpd on
            yes | sudo yum install wget -y
            yes | sudo yum install php php-mysql mysql -y
        Name: Learn 3Tier Architecture
        Enable Elastic IP
        Security Group: refer Wordpress-SG
        Subnet: refer Public subnet 1a
        VPC: refer Learn 3Tier Architecture
        Role: Refer Learn 3Tier Architecture
  ## Create SNS:
        IAM Role:
            EC2 use S3 with full access
            Name: Learn 3Tier Architecture
        Topic:
            Name: Learn_3Tier_Architecture
            Display Name: Learn 3Tier Architecture
            Subcription:
                Method: EMAIL
                Email: your email
  ## Create RDS:
    init id: WordpressDatabase
    db name: WordpressDatabase
    user: admin
    pass: 123456abc
    Create DB Subnet Group:
        Name: Learn 3Tier Architecture
        Az: 1a
        Subnets: the private subnet 1a
    


# NOTE: 
 ## Command line:
 aws cloudformation create-stack --stack-name blogger-example --template-body file://<your_path> --parameters ParameterKey=KeyName,ParameterValue=EC2 --capabilities CAPABILITY_IAM