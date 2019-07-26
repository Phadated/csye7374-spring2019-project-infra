Goal Of the Project
=========================
To run the Nodejs application in a hybrid cloud environment.

![Infrasturctureimage1](https://imagecsye6225.s3.amazonaws.com/Infra.png)

![Infrasturctureimage2](https://imagecsye6225.s3.amazonaws.com/infra2.JPG)

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
* Docker 18.09.2
* Helm 2.13.0

Installation
============
1. [Install Docker](https://docs.docker.com/install/)
2. [Install kops](https://github.com/kubernetes/kops/blob/master/docs/install.md)
3. [Install kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-binary-using-native-package-management)
4. [AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)
5. [Install Ansible](https://docs.ansible.com/ansible/2.4/intro_installation.html)
6. [Install Helm](https://helm.sh/docs/using_helm/)
7. Set up domain for the kubernestes cluster in Route53
example:

Repositories
============
1. Project Infrastructure -  __csye7374-spring2019-project-infra__ 
2. Main Web Application - __csye7374-spring2019-project-webapp__
3. Notifier Web Application - __csye7374-spring2019-project-notifier__
4. Lambda Function - __csye7374-spring2019-project-serverless__

## Create ECR Repository Using AWS CLI
aws ecr create-repository --repository-name csye7374

A separate AWS ECR docker registry must be created for each application and init containers.

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

## CI/CD Pipeline
CI/CD pipeline for all web applications using Jenkins.

## Service discovery
Both web applications will “discover” Kafka & Zookeeper endpoints using service discovery FQDN. 

## Ansible Roles and Playbooks (Infrastructure as code and Configuration Management)

``` clustermain.yml ``` - Creates the kubernestes cluster using kops in HA mode with 3 master nodes with each node in a separate availability zone. And will run minimum of 3 worker nodes in the cluster

``` autoscaler-playbook.yml ``` - Set autoscaling for the cluster (Auto scaling policy, Instance group, Configure Min, Max node count and cluster , Cluster autoscaler will run on master).

``` install-prometheus.yml ``` - Run prometheus server container  in the cluster using helm charts.

``` cluster-autoscaler.yml ``` - Creates Deployment, Serviceaccount, Role binding, Cluster Role, Role , Cluster Role Binding,

``` dashboard-teardown.yml ``` - Set up the kubernestes dashboard 

Dashboard can now be accessed locally via kubectl proxy at 
``` http://localhost:8001/api/v1/namespaces/kube-system/services/https:kubernetes-dashboard:/proxy/.```

``` fluentd.yml ``` - Run fluentd container  in the clusterusing helm charts.

``` install-grafana.yml ``` - Run Grafana server container in the cluster using helm charts.

``` install-jenkens.yml ``` - Run Jenkins server container  in the cluster using helm charts.

``` install-kafka.yml ``` - Run Kafka-Zookeepr containers  in the cluster using helm charts.

``` metrics-server-playbook.yml ``` - deploy metric server into cluster for Horizontal pod scaling

``` teardownmain.yml ``` - tear down the kubernestes cluster using kops command

``` tiller-playbook.yml ``` - Run tiller server containers in the cluster using helm charts.

``` portprometheus.yml``` - Port forwarding to prometheus server 

``` portgrafana.yml ``` - Port forwarding to grafana server 

## Setup AWS Organization
Enable organizations in your AWS account. Create a new account once organization has been setup. This new account will be where all resources that are not running on your Kubernetes cluster. 
Kubernetes cluster continue to run in the master account. Setup AWS VPCs in different account. Once both VPCs are setup, you will manually configure VPC peering for them.

Set up resource in the child AWS account using script and cloud formation scripts from csye7374-spring2019-project-infra/deploy/cloudformation/ directory.

## Init Containers 
- to Boostrap RDS
- to Create Kafka Topics
