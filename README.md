# Auto-Scaling-Load-Balancer-Integration-
This repository contains step-by-step instructions and configuration to:
✅ Create an Application Load Balancer <br>
✅ Launch an Auto Scaling Group <br>
✅ Automatically register EC2 instances from the ASG with the ALB <br>
✅ Distribute incoming traffic across healthy EC2 instances <br>
![1](https://github.com/user-attachments/assets/c41398e8-2cdf-49cf-92bd-897c8e63339f)
# Goto Console Click Ec2 and select target groups
click create target group,select target type ( In my case it is instances )
![2](https://github.com/user-attachments/assets/61894193-d9d9-4328-b74d-3cc44fb705b9)
# Select Ip address type,Vpc and protocol and port number
![3](https://github.com/user-attachments/assets/1bdcc6ea-c4da-4966-ab46-0b8072b65798)
# Select health check path
![4](https://github.com/user-attachments/assets/caa7c456-5969-4552-80b3-879887ded927)
#  Details of Target group
![5](https://github.com/user-attachments/assets/fb3031fb-34eb-4760-8ca4-e0cea01b4d29)
# Details of the Launch template 
# user Data
#!/bin/bash
# Update system and install Node.js, Git
yum update -y
curl -fsSL https://rpm.nodesource.com/setup_18.x | bash -
yum install -y nodejs git

# Switch to ec2-user's home directory
cd /home/ec2-user

# Clone your Node.js app repo (replace with your repo)
git clone https://github.com/johnpapa/node-hello.git
cd node-hello

# Install PM2 globally
npm install -g pm2

# Fix permissions so ec2-user owns the files
chown -R ec2-user:ec2-user /home/ec2-user

# Run PM2 commands as ec2-user to start app and setup startup
runuser -l ec2-user -c 'cd /home/ec2-user/node-hello && pm2 start index.js && pm2 startup systemd && pm2 save'

![6](https://github.com/user-attachments/assets/235d6cd8-68b6-4d20-888e-e7e70341ae64)
# Go to Loadbalancers and select Applican Load Balancer ( For web apps )
select the name,ip address type,vpc
![7](https://github.com/user-attachments/assets/19e20a8c-eaf9-4a19-b2a9-d94437f2973a)
# Select subnets,security groups
![8](https://github.com/user-attachments/assets/2e67668f-5af4-41a9-a26d-eb0405646a40)
# Select port number,protocol and target group 
![9](https://github.com/user-attachments/assets/4f2ee189-427c-4c7c-89ff-3c2095e98f4c)
# Summary of what we have created
![10](https://github.com/user-attachments/assets/e88ac53c-56f5-4547-a29b-ba33b70ac33f)
# Go to AutoScaling Group and click create new auto scaling group
give a name,select the launch template
![11](https://github.com/user-attachments/assets/02b1e175-836a-4707-8517-a9da318c7df4)
# Select a vpc,subnets,availability zone distribution
![12](https://github.com/user-attachments/assets/60992e4b-a6d5-40a5-916c-e20a36e3b2b7)
# Overview of Instance type requirements ( if want to change,then can change )
![13](https://github.com/user-attachments/assets/7768f9c4-2869-4592-a6cc-f7a5d52ff9a2)
# Attach this auto scaling to load balancer
So that it can auto scale based on the demand and this load balancer distributes the traffic equally.
![14](https://github.com/user-attachments/assets/51ef0987-e622-4eb4-a853-20070b829527)
# Specify the capacities and scaling policy
![15](https://github.com/user-attachments/assets/e343afa9-a390-4d14-94cb-856031179207)
# Health checks 
so that it will check the ec2 health after the grace period
![16](https://github.com/user-attachments/assets/7c3bb138-2e19-48c2-a7ad-e5f347b3aff4)
![17](https://github.com/user-attachments/assets/9c500b79-2a17-4cc5-95db-8a6e19a3c7df)
# Pictorial representation of how load balancer work
![18](https://github.com/user-attachments/assets/5122cddd-7df5-44d6-a04a-41cd7b21221a)
# Output of dns of load balancer
