# cloud-devops-high-availability
AWS CloudFormation IaC scripts to deploy a high-availability web app

## Description
The scripts are divided in three steps: network, roles and servers.

### Network
Deploys:
- VPC
- 2 Public Subnets
- 2 Private Subnets
- Internet Gateway: for the public subnets to have access to the internet
- NAT Gateway: for the private subnets to be able to download the necessary dependencies from the internet
- Route tables: to define subnets as public or private

### Roles
Deploys:
- IAM Role. The role allows EC2 instances to get objects from the S3 bucket udacity-demo-1 where the web app code is stored

### Servers
Deploys:
- Launch Configuration: to define how to get the web app up and running on an Apache web server within an EC2 instance
- Auto Scaling Group: to ensure there are at least 4 healthy servers hosting the web app for high availability
- Load Balancer: to distribute the load between the deployed servers and provide a single point of access
- Bastion host: to access the EC2 instances where the web app runs in a secure manner
- Security group: to allow access to the servers only on the necessary ports so that the environment is secure

## How to deploy the infrastructure above to AWS

1. Configure the AWS CLI tool as specified [here](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html).
2. Deploy the networking components:
```
aws cloudformation create-stack --stack-name UdagramNetwork --template-body file://network.json --parameters file://network-parameters.json --region=us-west-2
```
3. Deploy the IAM roles:
```
aws cloudformation create-stack --stack-name UdagramIAMRoles --template-body file://roles.json --parameters file://roles-parameters.json --region=us-west-2 --capabilities CAPABILITY_NAMED_IAM
```
4. Deploy the servers and the web app
```
aws cloudformation create-stack --stack-name UdagramServers --template-body file://servers.json --parameters file://servers-parameters.json --region=us-west-2
```
