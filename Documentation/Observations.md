<h1 align=center>Deployment 3 Observations</h1>

# Deploying to a customized VPC from a default VPC
## 1. Install Jenkins on an EC2
1	Create Amazon EC2 with Ubuntu image
	  a	select Key pair
	  b	under network settings create a security group or use an existing group and set ports
	  c	lauch instance
	
  2	Install Jenkins on the EC2
	  a	ssh into EC2 from the location of the Key pair (.pem file): $ssh -i {key}.pem @ubuntu{Public IPv4 DNS}
	  b	$curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee  /usr/share/keyrings/jenkins-keyring.asc > /dev/null
	  c	 $echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \ https://pkg.jenkins.io/debian-stable binary/ | sudo tee \ /etc/apt/sources.list.d/jenkins.list > /dev/null
	  d	$sudo apt-get update
	  e	 $sudo apt-get install jenkins
	  f	$sudo systemctl start jenkins
	
  3	Install Python on the EC2
	  a	$sudo apt install python3
	  b	$sudo apt install python3-venv
