---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---

# Deploying a Production-Style Spring Boot Application on AWS

#### Overview

In this workshop, you will learn how to deploy a containerized Spring Boot backend application (**CenFra-MS**) on a secure, observable, and automated AWS cloud infrastructure.

The architecture includes hosting the application in a public subnet on EC2, using a target group to route traffic via an Application Load Balancer (ALB), integrating custom domain DNS in Route 53 with SSL certificates from AWS Certificate Manager (ACM), distributing traffic via CloudFront CDN, and centralizing logs to Amazon CloudWatch.

#### Content

1. [Overview & Architecture](5.1-workshop-overview/)
2. [Prerequisites](5.2-prerequiste/)
3. [RDS Database](5.3-rds/)
4. [EC2 Deployment](5.4-ec2/)
5. [Target Group](5.5-targetgroup/)
6. [Application Load Balancer](5.6-load-balancer/)
7. [Route 53 & CloudFront CDN Integration](5.7-route53-cloudfront/)
8. [Amazon CloudWatch Logs & Monitoring](5.8-cloudwatch/)
9. [Clean Up CenFra-MS Resources](5.9-cleanup/)
