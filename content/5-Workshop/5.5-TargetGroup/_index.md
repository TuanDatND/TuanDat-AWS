---
title: "Target Group"
weight: 5
pre: " <b> 5.5. </b> "
---

# Configuring a Target Group for Application Load Balancer

In this section, we will create a **Target Group** to route requests from the Load Balancer to the EC2 instances running the backend application. We will also set up a **Health Check** mechanism to ensure the Load Balancer only sends requests to healthy servers.

### 1. Create a Target Group

* Log in to the AWS Management Console and access the **EC2** service.
* On the left navigation pane, scroll down to **Load Balancing** and select **Target Groups**.
* Click the **Create target group** button to start.

![Create Target Group - Empty Form](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-23%20173205.png)

Under **Basic configuration**:
* **Choose a target type**: Select **Instances** (since we will route traffic directly to EC2 virtual machines).
* **Target group name**: Provide a name for the Target Group (e.g., `aws-c8n`).
* **Protocol**: Select `HTTP`.
* **Port**: Enter `80` (The default port the Target Group will use to receive traffic from the Load Balancer).
* **VPC**: Make sure to select the VPC where your EC2 instances reside (e.g., `vpc-044c12820d72fc502`).
* **Protocol version**: Select **HTTP1**.

![Create Target Group - Basic Config](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-28%20101935.png)

### 2. Configure Health Check

Scroll down to the **Health checks** section. This is a crucial step for the Load Balancer to monitor the application's status.
* **Health check protocol**: Select `HTTP`.
* **Health check path**: Enter the API path used to ping the backend status. Instead of the default root path `/`, enter the custom path of the project: `/api/health`.

![Health Check Config](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-28%20102528.png)

Click **Next** to proceed to the register targets step.

### 3. Register Targets

At the **Register targets** step, the system will list the running EC2 instances within the selected VPC.
* Check the box next to the instance you want to add to the Target Group (e.g., Instance named `CenFra-MS`).
* In the **Ports for the selected instances** field, enter the port your backend application is actually running on (e.g., `8080`).
* Click the **Include as pending below** button. The instance will appear in the Review targets table below with a *Pending* status.

![Register Targets](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-28%20102707.png)

### 4. Review and create

At the final step, review all the configurations:
* Target type: `Instance`
* Protocol:Port: `HTTP: 80`
* Health check path: `/api/health`
* Ensure the instance is assigned to the correct port `8080`.

Once you have confirmed the information is correct, click the **Create target group** button at the bottom.

![Review Target Group](/images/5-Workshop/5.5-TargetGroup/Screenshot%202026-07-28%20102733.png)
