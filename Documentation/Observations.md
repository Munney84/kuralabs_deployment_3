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
