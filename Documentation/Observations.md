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

### 2. Create EC2 for Custom VPC
1. See "Create Amazon EC2 with Ubuntu image" steps above
2. Install the following packages: default-jre, python3-pip, python3.10-venv, nginx

## 3. Configure and connect Jenkins agent to Jenkins
1. Log into Jenkins using the IP address from your defaut VPC and port 8080
2. Select "Build Executor Status"
3. Select "New Node" and enter a node name (ex: awsDeploy) and choose "Permanent Agent" option
4. Configure Node to preferences (Usage = "only build with label", Launch Method = "launch agents via ssh", Host key verification strategy = "non verification strategy”, Availability = keep this agent online as much as possible)
5. Under "Launch Methods" select "Launch agents via SSH", and input public IP of the jenkins agent as the "Host"
6. Add Jenkins credentials and under "Kind" use "SSH Username with private key"

## 4. Create a Pipeline build in Jenkins
1. Prior to your build, SSH into the EC2 that has the Jenkins agent
2. Nano into "/etc/nginx/sites-enabled/default” file with sudo priviledges
3. Change the listening server from 80 to 5000 
<img width="593" alt="Screen Shot 2022-10-18 at 10 15 15 AM" src="https://user-images.githubusercontent.com/108026310/196455795-e705482d-d5da-4921-bf14-2e5bb2db88ab.png">

4. Change "location" to the following: 
<img width="735" alt="Screen Shot 2022-10-18 at 10 15 48 AM" src="https://user-images.githubusercontent.com/108026310/196455913-77ac791a-ab32-4525-a854-d45104e1319c.png">

5. Edit the Jenkinsfile in the github repository to match the following:
<img width="969" alt="Screen Shot 2022-10-18 at 10 11 30 AM" src="https://user-images.githubusercontent.com/108026310/196455026-48c49446-6be5-4e36-92ec-127b88f4aa77.png">

6. While logged into Jenkins, create a multibranch pipeline and connect it to your Github repo

## Additions to the pipeline (Slack Notificiations)
1. Add Slack Notification plugin to Jenkins (Navigate to "manage jenkins" and then "Manage plugins" to search for slack plugin
2. Configure the Jenkins integration using: https://my.slack.com/services/new/jenkins-ci choose the channel of choice to receive notifications
3. Save the "Integration Token Credential ID" as it is needed to when creating credentials 
4. Configure system to add Slack workspace "kura-labs" and add secret text credential using the "Integration Token Credential ID" from the above step. User ID can be found in the slack profile (copy starting with "U" 

## Issues
1. Host name in Node Config should be the Public IP of your EC2 located in the custom VPC’s public subnet
2. Must nano into nginx file with sudo proviledges to save edits
3. Jenkins plugin “Pipeline Keep Running Step” needed in order for the url to the flask app can open as it would close after the build
4. Restart nginx after edits for applied changes to take effect "$ sudo systemctl restart nginx"
5. Jenkins plugin “Pipeline Keep Running Step” needs to be downloaded in order for the url to the flask app can open as it would close after the build.
