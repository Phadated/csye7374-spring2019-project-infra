Goal Of the Project
=========================
To run the Nodejs application in a hybrid cloud environment.

![](https://imagecsye6225.s3.amazonaws.com/Infra.png)

Tech Stack Components
=========================
1.	Kubernetes Cluster (version 1.13 or later)
2.	FluentD Logging DaemonSet
3.	Prometheus Metrics Stack
4.	Helm
5.	Main Web Application
6.	AWS RDS
7.	AWS SNS
8.	AWS Lambda
9.	AWS SES
10.	AWS CloudWatch
11.	AWS S3
12.	AWS DynamoDB
13.	AWS VPC (and networking components)
14.	AWS Route53
15.	AWS ELB (for LoadBalancer Services)
16.	AWS EC2
17.	Notifier Web Application
18.	Apache Kafka (& Apache Zookeeper)
19.	CI/CD using Jenkins
20.	GitHub Repositories

Requirements
============
* kops Version 1.11.0
* Ansible 2.8
* Docker

Installation
============
1. Install Docker
2. Install kops
3. Install kubectl
4. AWS CLI

Set up domain for the kubernestes cluster in Route53
example: 

## Create ECR Repository Using AWS CLI
aws ecr create-repository --repository-name csye7374

## Deploying App to Kubernetes Cluster
•	A Node.js application is located in csye7374-spring2019-project-webapp/deploy/webapp/ directory.
•	A Docker file to build the image is in csye7374-spring2019-project-webapp/deploy/webapp/ directory.

## Build & Push Docker Container Image to Amazon ECR
### Retrieve the login command to use to authenticate your Docker client to your registry
```	aws ecr get-login --no-include-email --region us-east-1 ```

## Build Docker Image
``` docker build -t csye7374 . ```

## Tag Docker Image
``` docker tag csye7374:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/csye7374:latest ```

## Push Docker Image
``` docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/csye7374:latest ```

## Ansible Roles

``` clustermain.yml ``` - Creates the kubernestes cluster using kops and set up the kubernestes dashboard 

Dashboard can now be accessed locally via kubectl proxy at 
``` http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/.```
