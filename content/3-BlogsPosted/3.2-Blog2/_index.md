---
title: "Blog 2"
date: 2024-01-01
weight: 2
chapter: false
pre: " <b> 3.2. </b> "
---

# Kiro Steering Files: How to Make AI Always Comply with Security Standards on AWS?

Recently, I read a fascinating article on the **AWS Security Blog** about using **Kiro** and **Amazon Q** to strengthen security on AWS. I was particularly interested in:

* **Persistent Security Context via Steering Files.**

At first, it might not sound like anything special. But the more I read, the more I realized that this could be one of the most practical ways to solve the problem most of us face when using AI assistants every day.

AI is changing how we operate Cloud systems. From generating CloudFormation templates, writing IAM policies, analyzing Security Hub findings, to reviewing Infrastructure as Code (IaC), tools like Kiro or Amazon Q are saving Cloud and Security engineers a lot of time. But there's a major problem: **AI does not understand your organization's specific security standards.**

---

### A Very Realistic Scenario

Suppose you ask the AI: *“Create an S3 bucket for storing customer documents.”*

The AI can easily generate the bucket. But does it know that your organization requires:
* Enforcing Server-Side Encryption
* Blocking Public Access
* Enabling Versioning
* Enabling Access Logging
* Restricting access to VPC Endpoints?

Maybe it does, maybe it doesn't. And that is exactly where the risk lies.

---

### The AI Problem in Enterprise Environments

In an enterprise environment, deploying AWS resources requires more than just making them work. They must comply with:
* **Security Standards**
* **Compliance Requirements**
* **Internal Policies**
* **Governance Rules**

For example, a company might mandate:
* No creating IAM users (use IAM roles instead)
* No granting `AdministratorAccess` policy
* Encrypting all data by default
* Keeping CloudTrail enabled

If the AI doesn't know these rules, it might generate working infrastructure that fails security compliance audits.

---

### AWS's Proposed Solution: Kiro Steering Files

In a recent article, the AWS Security Blog introduced using **Kiro Steering Files** to provide a persistent security context for the AI. The idea is simple: instead of reminding the AI of security requirements in every prompt, we define them in a static rules file (Steering File) that the AI references during its operations.

#### Example: AWS Security Standards Steering File

```markdown
# AWS Security Standards

## IAM
- Apply least privilege principle
- Never attach AdministratorAccess policy

## S3
- Enable encryption by default
- Block all public access
- Enable versioning

## Logging
- CloudTrail must be enabled
- Enable AWS Config

## Networking
- Restrict inbound traffic
- Avoid 0.0.0.0/0 unless justified

## Compliance
- Follow CIS AWS Foundations Benchmark
```

With the Steering File, when you ask the AI to: *“Create an S3 bucket for application logs.”*

Instead of just creating a basic bucket resource, the AI will automatically append encryption configuration, versioning, bucket policies, and public access blocks from the start. This significantly reduces misconfigurations—one of the most common causes of cloud security breaches.

---

### Concept Illustration

![Kiro Steering Files Concept](/images/3-BlogsPosted/3.2-Blog2/kiro-steering-concept.png)
*Figure 1: Persistent security context via Steering Files to enforce secure IaC generation.*

---

### Multi-Account AWS Benefits

Many enterprises run separate Dev, Staging, Production, Security, and Logging accounts. Steering Files help the AI understand this account structure, IAM models, SCP restrictions, and logging requirements, leading to highly tailored, secure recommendations.

---

### Conclusion

Steering Files don't solve all security problems, but they ensure that your organization's security principles are a persistent part of the AI's context. In Cloud environments, getting the configuration right from the start is far more valuable than fixing security leaks later.

---

* **Original Reference Link**: [AWS Security Blog](https://aws.amazon.com/blogs/security/five-ways-to-use-kiro-and-amazon-q-to-strengthen-your-security-posture/)
* **Facebook Post Link**: [AWS Study Group Facebook Post](https://www.facebook.com/photo/?fbid=2034832500720323&set=gm.2176154606482833&idorvanity=660548818043427)