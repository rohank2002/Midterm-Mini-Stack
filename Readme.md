![Architecture Diagram]()
### Part 1: AWS Managed Database Instances Setup
In this section, you will configure MySQL and Redis as AWS Managed Services.  Following the configuration steps below:

### RDS MySQL Instance
1. Create DB Subnet Group
2. Name: mysql
3. Description: mysql
4. VPC: cmpe281
5. Add All Subnets Related to this VPC

### ** Need to have at least Two Private 
### ** Subnets accross two AZ' s

1. Create an RDS MySQL Instance
2. Select "Dev/Test - MySQL" Use Case
3. Select Version 5.5.61
4. Select "Only enable options eligible for RDS Free Usage Tier" 
5. Select Instance Type "db.t2.micro"
6. Instance Name: mysql
7. DB User: admin
8. DB Pass: midterm281
9. VPC: cmpe281
10. DB Subnet Group: mysql
11. Public accessibility: No
12. AZ Info: No Preference
13. Security Group: Chose or Create one with MySQL Port Open
14. DB Name: cmpe281
15. Port: 3306
16. IAM DB authentication: Disable
17. Disable auto minor version upgrade
18. Remaining Options: Use Defaults

### Install MySQL Client in your "Jump Box"
```sh
sudo yum update
Install mysql client
sudo yum install mysql
mysql -u admin -p -h <host>
```
### ElasticCache Redis Instance

1. Cluster Engine: redis
2. Name: redis
3. Description: redis
4. Version: 4.0.10
5. Port: 6379
6. Parameter group: default.redis4.0
7. Node type: cache.t2.micro
8. Number of Replicas: 2
9. Subnet Group: Create New
10. Subnet Group Name: redis
11. SG Description: redis
12. VPC: cmpe281
13. Subnets: select the two private subnets
14. Security Group: Chose or Create one with Redis Port Open

### Install Redis Client in your "Jump Box"
```sh
sudo yum install gcc
sudo yum install wget
wget http://download.redis.io/redis-stable.tar.gz
tar xvzf redis-stable.tar.gz
cd redis-stable
make
src/redis-cli -h <host> -p 6379
```
![RDS Security Groups]()
![ElastiCache Security Gropus]()

### Part 2: Deploy Gumball Cluster with an Internal ELB

* Modify the Go Gumball Code to connect to your Redis and MySQL Servers from Part 1. 
* Build the Code and deploy it to AWS (as two EC2 Instances running as Docker Hosts)
* Then put these two EC2 instances behind an Internal Elastic Load Balancer
#### NOTE:  All Instances in this section are in the "Private Network"

After you have the this up, test the Gumball API with the following CURL commands:
Note:  Replace "host" with the DNS name of your ELB.


