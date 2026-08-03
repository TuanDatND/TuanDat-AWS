---
title: "Workshop"
date: 2024-01-01
weight: 5
chapter: false
pre: " <b> 5. </b> "
---
{{% notice warning %}}
⚠️ **Note:** The information below is for reference purposes only. Please **do not copy verbatim** for your report, including this warning.
{{% /notice %}}

# Secure Hybrid Access to S3 using VPC Endpoints & CenFra-MS Setup

#### Overview

**AWS PrivateLink** provides private connectivity to AWS services from VPCs and your on-premises networks, without exposing your traffic to the Public Internet.

In this lab, you will learn how to create, configure, and test VPC endpoints that enable your workloads to reach AWS services without traversing the Public Internet.

In addition, you will deploy a containerized Spring Boot backend application (**CenFra-MS**) on a robust production-style AWS architecture utilizing EC2, Application Load Balancer (ALB), Route 53 DNS, CloudFront CDN, and CloudWatch.

#### Content

1. [Workshop overview](5.1-workshop-overview)
2. [Prerequiste](5.2-prerequiste/)
3. [Access S3 from VPC](5.3-s3-vpc/)
4. [Access S3 from On-premises](5.4-s3-onprem/)
5. [VPC Endpoint Policies (Bonus)](5.5-policy/)
6. [Clean up](5.6-cleanup/)
7. [EC2 Deployment](5.7-ec2/)
8. [Application Load Balancer](5.8-load-balancer/)
9. [Route 53 & CloudFront CDN Integration](5.9-route53-cloudfront/)
10. [Amazon CloudWatch Logs & Monitoring](5.10-cloudwatch/)
11. [Clean Up CenFra-MS Resources](5.11-cleanup/)
