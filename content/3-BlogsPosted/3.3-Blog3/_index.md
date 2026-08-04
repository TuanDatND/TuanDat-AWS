---
title: "Blog 3"
date: 2024-01-01
weight: 3
chapter: false
pre: " <b> 3.3. </b> "
---

# AWS Security Blog Deep Dive | Deconstructing DDoS Architecture and Traffic Data with Flow Logs in AWS Shield Advanced

In cybersecurity, protecting critical assets from DDoS attacks doesn't just stop at setting up a strong firewall and leaving the system to handle it automatically. To truly master the system, we need packet-level visibility to analyze attacker behavior.

The article *“Gain visibility into DDoS attacks with flow logs in AWS Shield Advanced”* by Ken Kitts provides a deep look at how AWS addresses this. Here is a technical breakdown focusing on the delivery architecture and data flows.

---

### 1. The Nature of Flow Logs in Mitigation

When an infrastructure-layer DDoS attack occurs, the hacker's goal is to saturate bandwidth or exhaust connection tables, causing packet loss and timeouts. Instead of just offering aggregate CloudWatch metrics (which lack detail) or forcing engineers to piece data together after the event, AWS Shield Advanced generates **attack flow logs**.

* **Logging Interval**: Logs are continuously captured and published in 5-minute increments during and after an attack.
* **File Size Limit**: The maximum size of each log file is 75 MB. If the flood of data reaches this limit before 5 minutes, AWS automatically closes the file, publishes it, and opens a new log file.
* **Supported Formats**: Formats like JSON, plain text, W3C, and Parquet are supported, making them highly flexible for data pipelines.
* **Note**: Currently, Shield Advanced supports flow logs for resources protected by Elastic IPs (EIPs). Support for other resources (CloudFront, ALB, Route 53) will be rolled out later.

---

### 2. Log Record Schema

AWS captures detailed metadata. This JSON data can be retrieved via APIs to build custom dashboards using HTML/CSS, helping visualize network traffic in real time. Key fields include:

* `protection_arn`: The ARN identifier of the Shield protection.
* `srcaddr` / `dstaddr` & `srcport` / `dstport`: IP addresses and ports of the attack source and destination.
* `protocol` & `tcp_flags`: Protocol type (TCP, UDP, ICMP) and TCP flags (e.g. SYN, ACK) to identify flood techniques like SYN Flood.
* `packets` / `bytes`: Volume of traffic in the aggregation window.
* `action`: Indicates whether Shield blocked (`Block`) or allowed (`Allow`) the traffic.
* `location` & `srccountry`: The AWS edge location where traffic entered and the 2-letter country code of the source IP.

---

### 3. Reference Architecture Diagram

![AWS Shield Advanced Flow Logs Reference Architecture](/images/3-BlogsPosted/3.3-Blog3/shield-flowlogs-architecture.png)
*Figure 1: Decoupled delivery architecture for Shield Advanced attack flow logs.*

---

### 4. Log Delivery Architecture

AWS decouples log delivery using three independent logical entities, allowing log destination sharing across accounts and regions:

* **DeliverySource**: Represents the Shield Advanced protection ARN (not the ARN of the EC2 or ALB resource itself).
* **DeliveryDestination**: The final storage destination, which can be an Amazon S3 bucket (for Athena queries), a CloudWatch Logs log group (for real-time Insights), or Amazon Data Firehose (to stream to third-party SIEMs).
* **Delivery**: The logical link that binds the Source and Destination.

---

### 5. Technical Implementation via AWS CLI

DevOps/Security engineers need IAM permissions like `logs:PutDeliverySource`, `logs:PutDeliveryDestination`, and `logs:CreateDelivery`. The setup flow is:

#### Step 1: List Shield protections to get the protection ARN (run in us-east-1 since Shield is a global service).
```bash
aws shield list-protections --region us-east-1
```

#### Step 2: Create the Delivery Source.
```bash
aws logs put-delivery-source \
--name my-shield-delivery-source \
--resource-arn <protection-arn> \
--log-type FLOW_LOGS \
--region us-east-1
```

#### Step 3: Create the Delivery Destination pointing to your S3, Log Group, or Firehose.
```bash
aws logs put-delivery-destination \
--name my-shield-delivery-destination \
--output-format plain \
--delivery-destination-configuration '{"destinationResourceArn":"<resource-arn>"}' \
--region us-east-1
```

#### Step 4: Associate the source and destination (Create Delivery).
```bash
aws logs create-delivery \
--delivery-source-name my-shield-delivery-source \
--delivery-destination-arn <delivery-destination-arn> \
--region us-east-1
```

---

### Summary

The best part of AWS Shield Advanced Flow Logs is cross-account and cross-region centralization. You can aggregate logs from multiple AWS accounts into a single central S3 bucket, then query with Athena and visualize with QuickSight. Having near-real-time tcp_flags analysis without installing agents or modifying applications is the core strength of cloud-native security.

---

* **Original Reference Link**: [AWS Security Blog](https://aws.amazon.com/vi/blogs/security/gain-visibility-into-ddos-attacks-with-flow-logs-in-aws-shield-advanced/)
* **Facebook Post Link**: [AWS Study Group Facebook Post](https://www.facebook.com/groups/awsstudygroupfcj/posts/2211111629653797?locale=vi_VN)