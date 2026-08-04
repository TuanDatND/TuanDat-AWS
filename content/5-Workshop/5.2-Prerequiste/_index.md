---
title : "Prerequisites"
date : 2024-01-01 
weight : 2 
chapter : false
pre : " <b> 5.2. </b> "
---

Before beginning the deployment workshop, ensure you have completed the following prerequisites:

#### 1. AWS Account & User
* An active **AWS Account**.
* An IAM user or role with permissions to provision EC2 instances, Application Load Balancers, Route 53 Records, CloudFront Distributions, and CloudWatch Log Groups.

#### 2. Network Infrastructure
* A **Virtual Private Cloud (VPC)** configured with:
  * At least two **Public Subnets** (in different Availability Zones) for the Application Load Balancer and public access.
  * A configured **Internet Gateway** attached to the VPC.

#### 3. Custom Domain
* A registered domain name managed via **Amazon Route 53** (e.g., `tuandat.space`).
* Access to create hosted zone records.

#### 4. Container Image
* The **CenFra-MS** application code should be packaged as a Docker image and pushed to a public registry (e.g. Docker Hub `tuandat/cenframs-backend:latest` or AWS ECR).
* A prepared `docker-compose.yml` file to pull and start the application container.