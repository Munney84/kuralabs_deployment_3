<h1 align=center>Deployment 3 Observations</h1>

# Deploying to a customized VPC from a default VPC
## 1. Installing Jenkins Server on an EC2
### 1. Create Amazon EC2 with Ubuntu image
1. select Key pair
2. under network settings create a security group or use an existing group and set ports
3. launch instance
### 2. Install Jenkins on the EC2
1. ssh into EC2 from the location of the Key pair (.pem file): $ssh -i {key}.pem @ubuntu{Public IPv4 DNS}
2. $curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
3.	$echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \ https://pkg.jenkins.io/debian-stable binary/ | sudo tee \ /etc/apt/sources.list.d/jenkins.list > /dev/null
4. $sudo apt-get update
5. $sudo apt-get install jenkins
6. $sudo systemctl start jenkins **
7. $sudo apt install python3
8. $sudo apt install python3-venv

## 2. Create an EC2 in Public Subnet of a Custom VPC
### 1. Create VPC
1. On AWS go to VPC section and create VPC (select VPC only on next page)
2. Label your VPC in the name tag section
3. Adjust IPv4 CIDR as needed (172.25.0.0/16)
4. Accept default settings number of public and private subnets, NAT gateway and VPC endpoints
5. Click Create VPC
6. Click Subnets and create subnet using the VPC ID you created
7. Enter subnet name and select availalability zone of choice
8. Create subnet

### 2. Create EC2 for 
